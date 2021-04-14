# react hook相关

#### [react作者Dan-useEffect 完整指南](https://overreacted.io/zh-hans/a-complete-guide-to-useeffect/)  
  从"心智模型"的角度讲解hook  
  1.如何用useEffect模拟componentDidMount生命周期?  
  2.如何正确地在useEffect里请求数据?[]又是什么?    
  3.应该把函数当做effect的依赖吗?  
  4.为什么有时候会出现无限重复请求的问题?  
  5.为什么有时候在effect里拿到的是旧的state或prop呢?  
#### [Dan-为什么hook要顺序调用](https://overreacted.io/zh-hans/why-do-hooks-rely-on-call-order/)  
  以作者角度去分析hook的设计,例如：    
  1.为什么多次调用useState  
  2.为什么不采用key区分state变量来摆脱hook顺序调用  
  3.支持hook之间传值  
  ...
#### [蚂蚁RichLab前端团队-hook上手指南](https://zhuanlan.zhihu.com/p/92211533)  
  1.hooks链表结构  
  2.useState更新数据和setState不同的是，前者会与old state做merge，我们只需把更改的部分传进去，但是useState则是直接覆盖  
  3.useEffect callback是在组件被渲染为真实DOM后执行（所以可以用于DOM操作）-- **对比componentDidMount 在commit的layout阶段执行而useEffect callback会在commit阶段完成后异步调用**,useLayout同didMount调用时期一致  
#### **《hook实践指南》**  
  - [网易云音乐团队-hook最佳实践](https://juejin.cn/post/6844904165500518414)   
  - [常用hook使用方法及注意事项](https://juejin.cn/post/6844903985338400782)`(推荐)`  
  - [腾讯IMWeb团队-hook最佳实践](https://juejin.cn/post/6888597510399623175)

#### **《hook源码解读》**
  - [手写hook](https://juejin.cn/post/6872223515580481544)-虽然与react实现方式不同，但可以有助于理解常用hook的工作模式  
  - [饿了么团队-hook源码解读](https://zhuanlan.zhihu.com/p/91935584)`(推荐)`  
    1.函数组件在「第一次执行」时所使用的 Hook (useXXX) 指向的是对应的 mountXXX，而在更新时，Hook 指向的是对应的 updateXXX  
    2.函数组件在渲染时将 Hook 按照调用顺序以链表的形式挂在 Fiber Node 上面。得到的链表如下所示，其中 Fiber Node 的属性 memoizedState 用来指向Hook 链表的第一个节点，Hook 的属性 memoizedState 用来存放 state 值或 effect回调函数  
    3.顺序调用的流程  
    4.hook如何通过闭包实现状态管理(讲解的很清晰)  
  - [hook运行机制-配合饿了么的文章食用](https://juejin.cn/post/6944863057000529933)  
    ![](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d99a12ad708647d4bfd075a59d518c8b~tplv-k3u1fbpfcp-watermark.image?imageslim ':size=60%')  
  - [useState源码解析](https://zhuanlan.zhihu.com/p/200855720)

#### [hook使用时一些值得关注的点](https://juejin.cn/post/6844904176003055630)
  文章中的例子还不错  
  1.函数式组件每一次渲染都会**从上到下将所有内容重新执行一次，如果有变量，就会创造新变量**  
  2.useState初始值惰性更新-useState(func())这样写的话会导致每次都只写func()获取初始值  
  注意区分useState(func())和useState(func)：useState(func()) ❎ useState(func)✅     
#### [hook vs 生命周期](https://juejin.cn/post/6844903901620092941)  
  前半部分hook *"模拟"* 生命周期可以帮助class component =》functional component更好的过度，**但要注意"心智模型"!**  
  1.componentDidCatch and getDerivedStateFromError：目前还没有这些方法的 Hook 等价写法, 所以一定程度上 class component并不能完全被替代  
  2.这篇文章并没有准确提到如何模拟componentDidUpdate(可以用useRef"模拟")  
#### [阿里CRO团队-hook所带来的变化](https://juejin.cn/post/6901956745417981959)
  对比总结了在开发过程中,hooks 所带来的变化以及如何去应对这种变化,有助于理解Dan所说的"心智模型"
#### 《custom hook》
  - [custom hook设计规范](https://juejin.cn/post/6890738145671938062)

#### [知乎上react hook vs vue3 composition api的讨论](https://www.zhihu.com/question/378861485/answer/1125724740)
