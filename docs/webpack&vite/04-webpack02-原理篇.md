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