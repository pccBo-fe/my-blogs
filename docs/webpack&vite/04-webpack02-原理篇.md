# webpack-原理

[《深入浅出webpack》-原理 ](http://webpack.wuhaolin.cn/5%E5%8E%9F%E7%90%86/)  
[webpack优秀博文合集](https://github.com/webpack-china/awesome-webpack-cn)

- [ ] webpack源码流程
- [ ] [tapable](https://github.com/webpack/tapable) 
- [ ] Module Federation
  - https://zhuanlan.zhihu.com/p/120462530
  - https://zhuanlan.zhihu.com/p/352936804
  - https://segmentfault.com/a/1190000024449390
- [ ] webpack构建流程
- [ ] loader 
- [ ] plugin
- [ ] compiler vs compilation
- [ ] webpack 打包结果
  - webpack 打包输出打是一个 IIFE(匿名闭包) 
  - modules  是一个数组，每一项是一个模块初始化函数  
  - 使用 __webpack_require() 来家在模块，返回 module.exports  
  - 通过 __webpack_require__(__webpack_require__.s = 0); 启动程序  
  - 异步加载会动态创建script标签, 发起http请求

Webpack 核心原理: https://zhuanlan.zhihu.com/p/363928061


Webpack 如何实现热更新的呢？首先是建立起浏览器端和服务器端之间的通信，浏览器会接收服务器端推送的消息，如果需要热更新，浏览器发起http请求去服务器端获取打包好的资源解析并局部刷新页面。  

在HMR（热更新）方面，当改动了一个模块后，vite仅需让浏览器重新请求该模块即可，不像webpack那样需要把该模块的相关依赖模块全部编译一次，效率更高  


[面试向](https://juejin.cn/post/6943468761575849992)

[webpack原理知识汇总](https://mp.weixin.qq.com/s/KlxCK9KsY6KHw67YPD4Ftw)

#### [loader源码](https://blog.csdn.net/vv_bug/article/details/107722103) `推荐`
  [还有一篇必看文章](https://segmentfault.com/a/1190000021657031)
  非常好的文章，写webpack loader必看

#### [plugin源码](https://segmentfault.com/a/1190000021593923) `推荐`