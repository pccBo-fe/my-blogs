# 对比15版本与16+版本的区别  
#### 生命周期
`v15`  
![](https://user-images.githubusercontent.com/24861316/46216126-7bc48e80-c371-11e8-86e3-2565fd251508.png ':size=80%')  
`v16.3`  
![](https://user-images.githubusercontent.com/24861316/68308912-96c30f80-00e8-11ea-9317-126940a5cadb.png ':size=80%')  
`v16.4+`
getDerivedStateFromProps无论是Mounting还是Updating，也无论是因为什么引起的Updating，全部都会被调用
![](https://user-images.githubusercontent.com/24861316/68309077-dee23200-00e8-11ea-8fa3-7a8f4e5fec4c.png ':size=80%')  
16+版本还有两个错误处理钩子：  
`static getDerivedStateFromError` 从错误中获取 state  
`componentDidCatch` 捕获错误并进行处理
#### setState区别汇总
  ##### 1.同步or异步  
  `v15`是通过isBatchingUpdates来判断的，具体如下:  
  - 合成事件&生命周期中会调用batchUpdates,isBatchingUpdates设置为true,后续执行会被包裹在transaction中进行执行,入队dirtyComponents, 函数内容执行完后触发flush update中间件close，进行flushBatchUpdate消费dirtyComponents,然后执行reset中间件close将isBatchingUpdates重置为false
  - setTimeout等宏任务执行时isBatchingUpdates已经被重置，所以行为上是同步  

?> 翻译版：
  ``` js 
  increment = () => {
  // 进来先锁上
    isBatchingUpdates = true
    console.log('increment setState前的count', this.state.count)
    this.setState({
      count: this.state.count + 1
    });
    console.log('increment setState后的count', this.state.count)
    // 执行完函数再放开
    isBatchingUpdates = false
  }
 
  reduce = () => {
    // 进来先锁上
    isBatchingUpdates = true
    setTimeout(() => { // 宏任务，执行时 isBatchingUpdates = false
      console.log('reduce setState前的count', this.state.count)
      this.setState({
        count: this.state.count - 1
      });
      console.log('reduce setState后的count', this.state.count)
    },0);
    // 执行完函数再放开
    isBatchingUpdates = false
  }
  ```

!> `v16+` **在不同的启动模式下表现不同**：  
  -legacy：`setXXX hook dispatch本质在执行dispatchAction/setState执行enqueueSetState`,进而进入scheduleUpdateOnFiber中的`lane === synclane`,通过全局上下文executionContext/batchedContext(不同版本代码有点小区别)来判断是否处于批量更新阶段: 合成事件&生命周期会修改全局上下文(**不再通过事务去执行**)进而批量更新, setTimeout会"摆脱"此束缚进而执行flushSyncCallbackQueue,出现"同步"行为(大体上类似于v15版本的"翻译版")  
  -concurrent：**由于`lane ≠ synclane`,不会再出现"同步"行为, 改为单个时间分片内批量更新**
  - [concurrent模式下的setState](https://segmentfault.com/a/1190000024560483) `待看`
  - [concurrent模式硬核加强版文章](https://segmentfault.com/a/1190000022942008) `待看`
  ##### 2.useState vs setState
  - useState自带浅比较(Object.is), setState使用 scu or pureComponent  
  - **useState不会自动合并到旧的state而是直接覆盖, setState会**  
   ```js 
   setXXX(prev => ({...prev, ...updateValues}))
   ```
#### 事件系统
  - v17前统一委托在document,v17+改为root 节点  
  - v17放弃事件池,为每一个合成事件创建新的对象  
  - v17支持了原生捕获事件  
#### context vs React.createContext
  [源码分析](https://xie.infoq.cn/article/d56577c78e76508722e37025f)
  [context使用技巧](https://zhuanlan.zhihu.com/p/50336226)`必看推荐`  跟性能优化相关  
