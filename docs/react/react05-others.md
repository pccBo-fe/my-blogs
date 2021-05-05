# react 其他核心知识合集 
`15版本相关 & 事务/事件机制 & 性能优化 等`

#### transaction（15）
  结合setState理解更加
  - [结合setState理解事务机制](https://juejin.cn/post/6844903511478697998)  
  - [事务机制-中间件](https://www.yuque.com/stickmyc/react-analysis/gbbysx)  
  中间件机制可以对比koa,redux dispatch,webpack loader去理解  

#### 事件系统
  - [原生事件系统](https://zh.javascript.info/bubbling-and-capturing)
    先了解原生的工作流程，才能更好的理解react模拟捕获&冒泡的过程  
    1.捕获阶段（Capturing phase）—— 事件（从 Window）向下走近元素  
    2.目标阶段（Target phase）—— 事件到达目标元素  
    3.冒泡阶段（Bubbling phase）—— 事件从元素上开始冒泡  
    eg: 点击 span，事件首先通过祖先链向下到达元素（捕获阶段），然后到达目标（目标阶段），最后上升（冒泡阶段），在途中调用处理程序  
  - [网易云音乐-事件系统工作原理](https://juejin.cn/post/6909271104440205326)
  - [事件系统源码分析1](https://juejin.cn/post/6844903538762448910)
  - [事件系统源码分析2](https://github.com/yangdui/blog/issues/20)  
  1.事件注册: mount阶段`completework`统一委托到document层  
  2.事件触发：react模拟冒泡/捕获, 并且会打开批量更新 
  - [事件系统源码分析3](https://juejin.cn/post/6844903988794671117)
  - [react合成事件经典问题1-无法阻止原生冒泡](https://github.com/facebook/react/issues/4335)`v17已修复`
  - react合成事件经典问题2-setTimeout中无法获取event`v17已修复`
  ```js
  function handleChange(e) {
    // This won't work because the event object gets reused.
    setTimeout(() => {
      console.log(e.target.value); // v17前拿不到，为了复用该对象已被重置
    }, 100);
  }
  ```

#### setState相关
  - [个人整理-setState同步or异步](https://wiki.n.miui.com/pages/viewpage.action?pageId=549096496)  
    setState 是同步的还是异步的取决于之前有没有执行过 batchedUpdates ，如果有就是异步的。如果没有执行过，就是同步的  
    1.生命周期中的 setState 和 React 合成事件的 setState 是异步的  
    2.setTimeout 和 原生事件（因为没有初始调用batchUpdates） 中的 setState 是同步的 
  - [16+setState批处理源码](https://zhuanlan.zhihu.com/p/56507101)

#### componentDidCatch(error, info) vs static getDerivedStateFromError(error)
  1.调用时机不同,getDerivedStateFromError渲染时调用,因此为static;componentDidCatch commit阶段调用,因此可以执行副作用(如log等)  
  2.getDerivedStateFromError 返回值 merge state,用法如下:   
  ```js
  class ErrorBoundary extends React.Component {
    constructor(props) {
      super(props);
      this.state = { hasError: false };
    }

    static getDerivedStateFromError(error) {
      // 更新 state 使下一次渲染可以显降级 UI
      return { hasError: true };
    }

    render() {
      if (this.state.hasError) {
        // 你可以渲染任何自定义的降级  UI
        return <h1>Something went wrong.</h1>;
      }

      return this.props.children;
    }
  }
  ```
  3.这两种方法(&getDerivedStateFromProps)暂时都没有对应的hook(react正在做),所以从这个角度来说fc暂时无法完全替代cc

#### [react的异常处理](https://github.com/HuJiaoHJ/blog/issues/12)

#### [react的性能优化](https://github.com/brickspert/blog/issues/36)
  [另一篇总结不错的文章](https://juejin.cn/post/6844904000043614222)
  React 的优化方向：减少 render 的次数；减少重复计算。  
  - useState自带浅比较
  - 使用useCallback
  - 谨慎使用context(无法阻止无用的渲染)
    [context使用技巧](https://zhuanlan.zhihu.com/p/50336226)`必看推荐`  
    这篇文章介绍的提取不需要渲染的部分转为子组件并采用this.props.children的方式非常值得推荐  
  - redux 精细化依赖
  - 合理的拆分组件其实也是可以做性能优化的，你这么想，如果你整个页面只有一个大的组件，那么当 props 或者 state 变更之后，需要 reconciliation 的是整个组件，其实你只是变了一个文字，如果你进行了合理的组件拆分，你就可以控制更小粒度的更新。