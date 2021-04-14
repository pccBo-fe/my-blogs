# vue响应式原理

### defineProperty
```js
// 触发更新视图
function updateView() {
  console.log('视图更新')
}

// 重新定义数组原型
const oldArrayProperty = Array.prototype
// 创建新对象，原型指向 oldArrayProperty ，再扩展新的方法不会影响原型
const arrProto = Object.create(oldArrayProperty);
['push', 'pop', 'shift', 'unshift', 'splice'].forEach(methodName => {
  arrProto[methodName] = function () {
    updateView() // 触发视图更新
    oldArrayProperty[methodName].call(this, ...arguments)
    // Array.prototype.push.call(this, ...arguments)
  }
})

// 重新定义属性，监听起来
function defineReactive(target, key, value) {
  // 深度监听
  observer(value)

  // 核心 API
  Object.defineProperty(target, key, {
      get() {
          return value
      },
      set(newValue) {
          if (newValue !== value) {
              // 深度监听
              observer(newValue)

              // 设置新值
              // 注意，value 一直在闭包中，此处设置完之后，再 get 时也是会获取最新的值
              value = newValue

              // 触发更新视图
              updateView()
          }
      }
  })
}

// 监听对象属性
function observer(target) {
  if (typeof target !== 'object' || target === null) {
      // 不是对象或数组
      return target
  }

  // 污染全局的 Array 原型
  // Array.prototype.push = function () {
  //     updateView()
  //     ...
  // }

  if (Array.isArray(target)) {
      target.__proto__ = arrProto
  }

  // 重新定义各个属性（for in 也可以遍历数组）
  for (let key in target) {
      defineReactive(target, key, target[key])
  }
}

// 准备数据
const data = {
  name: 'zhangsan',
  age: 20,
  info: {
      address: '北京' // 需要深度监听
  },
  nums: [10, 20, 30]
}

// 监听数据
observer(data)

// 测试
// data.name = 'lisi'
// data.age = 21
// // console.log('age', data.age)
// data.x = '100' // 新增属性，监听不到 —— 所以有 Vue.set
// delete data.name // 删除属性，监听不到 —— 所有已 Vue.delete
// data.info.address = '上海' // 深度监听
data.nums.push(4) // 监听数组
```
### proxy
```js
// 创建响应式
function reactive(target = {}) {
  if (typeof target !== 'object' || target == null) {
    // 不是对象或数组，则返回
    return target
  }

  // 代理配置
  const proxyConf = {
    get(target, key, receiver) {
      // 只处理本身（非原型的）属性
      const ownKeys = Reflect.ownKeys(target)
      if (ownKeys.includes(key)) {
        console.log('get', key) // 监听
      }

      const result = Reflect.get(target, key, receiver)
  
      // 深度监听
      // 性能如何提升的？
      return reactive(result)
    },
    set(target, key, val, receiver) {
      // 重复的数据，不处理
      if (val === target[key]) {
        return true
      }

      const ownKeys = Reflect.ownKeys(target)
      if (ownKeys.includes(key)) {
        console.log('已有的 key', key)
      } else {
        console.log('新增的 key', key)
      }

      const result = Reflect.set(target, key, val, receiver)
      console.log('set', key, val)
      // console.log('result', result) // true
      return result // 是否设置成功
    },
    deleteProperty(target, key) {
      const result = Reflect.deleteProperty(target, key)
      console.log('delete property', key)
      // console.log('result', result) // true
      return result // 是否删除成功
    }
  }

  // 生成代理对象
  const observed = new Proxy(target, proxyConf)
  return observed
}

// 测试数据
const data = {
  name: 'zhangsan',
  age: 20,
  info: {
    city: 'beijing',
    a: {
      b: {
        c: {
          d: {
            e: 100
          }
        }
      }
    }
  }
}

const proxyData = reactive(data)
```