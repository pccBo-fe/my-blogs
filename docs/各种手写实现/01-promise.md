# promise相关手写实现

#### 具备取消功能的promise
> promise为状态机，本身无取消状态，这里所说的取消实际上是取消then后续的处理逻辑，并不会真的取消请求，所以在此场景下需要服务端对应接口幂等
```js
let count = 1
const cancelablePromise = (p) => {
  let canceled = false
  let reason = 'cancel reason'

  const wrappedPromise = new Promise((res, rej) => {
    // 写法1
    p.then((val) => {
      console.log('thenthen', val)
      canceled ? rej({ reason, val }) : res(val)
    }).catch(err => {
      console.log('catchcatch', err)
      canceled ? rej({ reason, val: err }) : res(err)
    })
    // 写法2
    // p.then(
    //   val => {
    //     console.log('thenthen', val)
    //     canceled ? rej({ reason, val }) : res(val)
    //   },
    //   err => {
    //     console.log('catchcatch', err)
    //       canceled ? rej({ reason, val: err }) : res(err)
    //   }
    // )
  })

  return {
    cancel (cancelReason) {
      reason = cancelReason
      canceled = true
    },
    promise: wrappedPromise
  }
}

const myPromise = new Promise((res, rej) => setTimeout(() => rej(count++), 1000))
const cancelable = cancelablePromise(myPromise)

cancelable.promise
  .then(() => console.log(count))
  .catch(({ reason, val }) => console.log('reason:', reason, '; val:', val)) // reason: 某些原因 ; val: 1

cancelable.cancel('某些原因')
setTimeout(() => console.log(count), 2000) // count 2 可以看出实际上count还是变成了2
```

#### 具备并行限制的promise调度器
> 思路: 队列 + 递归
```js
// 思路: 递归 + 队列
class Scheduler {
  constructor(maxCount = 3) {
    this.queue = [] // promise队列
    this.maxCount = maxCount // 并行数量
    this.runningCount = 0 // 正在执行的数量
  }

  // 添加队列
  push(promiseCreator) {
    // console.log('====push')
    this.queue.push(promiseCreator) // 这里添加的是一个promise creator函数，并不是promise本身
  }
  // 触发器
  start() {
    for(let i = 0; i < this.maxCount; i ++) {
      this.task()
    }
  }
  // 调度函数
  task() {
    // 处理边界条件
    if (!this.queue.length || this.runningCount >= this.maxCount) {
      return
    }
    // 增加正在执行的数量
    this.runningCount++
    console.log('正在执行的数量:', this.runningCount)
    // 从队列中取出函数执行
    this.queue.shift()().then((val) => {
      console.log('又结束了一个，还剩:', this.queue.length)
      console.log(val)
      // 已经有执行完毕的promise，继续调度
      this.runningCount--
      this.task() // 递归
    }).catch(val => {
      console.log('catch', val)
      this.runningCount--
      this.task() // 递归
    })
  }
}

const scheduler = new Scheduler(2)

const gen = (time, order) => new Promise((res, rej) => {
  setTimeout(() => res(order), time)
})
const genErr = (time, order) => new Promise((res, rej) => {
  setTimeout(() => rej(order), time)
})

scheduler.push(() => gen(1300, 1))
scheduler.push(() => genErr(800, 2))
scheduler.push(() => gen(1500, 3))
scheduler.push(() => gen(300, 4))
scheduler.push(() => genErr(2000, 5))
scheduler.push(() => genErr(600, 6))
scheduler.push(() => gen(200, 7))
scheduler.push(() => gen(600, 8))

scheduler.start()
// 结果:
// 正在执行的数量: 1
// 正在执行的数量: 2
// catch-order: 2 还剩: 6
// 正在执行的数量: 2
// order: 1 还剩: 5
// 正在执行的数量: 2
// order: 4 还剩: 4
// 正在执行的数量: 2
// order: 3 还剩: 3
// 正在执行的数量: 2
// catch-order: 6 还剩: 2
// 正在执行的数量: 2
// order: 7 还剩: 1
// 正在执行的数量: 2
// catch-order: 5 还剩: 0
// order: 8 还剩: 0
```