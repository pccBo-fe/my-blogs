# react fiber相关

#### [个人整理-workLoop流程](https://xiaomi.f.mioffice.cn/docs/dock4PzmhM1a1YAmLaN8tRUFIQc)
#### [fiber reconcile流程](http://echizen.github.io/tech/2019/04-06-react-fiber)`推荐`
  可以搭配上一篇文章加深理解,`这部分可以接着看hook章节中饿了么团队的文章去进一步体会函数式组件的执行过程`,其中：    
  函数式组件mount阶段beginwork会执行函数并依次执行hook挂载到memorizedState中,调用setXXXhook时会更新对应的hook链表并触发rerender(**setXXX dispatch本质上是dispatchAction闭包,会触发scheduleUpdateOnFiber(判断是否批量更新逻辑也在此处)**)进入update阶段，update阶段会在beginwork中再次执行函数获取最新的state props等已达到fiber node更新产生effectList
#### [网易云音乐-fiber源码解析](https://juejin.cn/post/6859528127010471949)  
#### [饿了么-workLoop原理](https://zhuanlan.zhihu.com/p/74344654)  
#### [涂鸦智能-fiber源码解析](https://tech.tuya.com/react-fiberyuan-ma-li-jie/)  
#### [build your own react](https://pomb.us/build-your-own-react/)  
  由浅入深理解react fiber架构
#### [理解react fiber](http://www.ayqy.net/blog/dive-into-react-fiber/) 
  后面总结部分写的很好  
#### [从作者角度去分析fiber架构](http://taoweng.site/index.php/archives/262/)`推荐`
#### [政采云- fiber是如何实现更新过程可控的](https://juejin.cn/post/6911681589558640654)`推荐`
  ![](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/6051a82ff5604046a11a80c6fe0d4d00~tplv-k3u1fbpfcp-zoom-1.image ':size=40%')  
  从源码角度重点讲述react如何挂起,恢复,终止任务的:  
  在一次任务结束后返回该处理节点的子节点或兄弟节点或父节点。只要有节点返回，说明还有下一个任务，下一个任务的处理对象就是返回的节点。通过一个全局变量记住当前任务节点，当浏览器再次空闲的时候，通过这个全局变量，找到它的下一个任务需要处理的节点恢复执行。就这样一直循环下去，直到没有需要处理的节点返回，代表所有任务执行完成  
  关于fiber的思考：  
  1.能否使用生成器（generater）替代链表  
  2.Vue 是否会采用 Fiber 机制来优化复杂页面的更新  
#### [双缓冲策略图解](https://github.com/suoutsky/three-body-problem/issues/123)
#### [知乎上关于concurrent模式的讨论](https://www.zhihu.com/question/434791954)  
#### [为什么采用postMessage实现时间分片](https://juejin.cn/post/6953804914715803678) 
  - [ ] 待看
#### [为什么采用lane做任务优先级](https://juejin.cn/post/6951206227418284063)
  - [ ] 待看