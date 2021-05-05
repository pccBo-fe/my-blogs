# babel

#### [babel介绍](https://juejin.cn/post/6854573211586150413)
  对babel进行了简明清晰地介绍  
  其中关于polyfill以transform-runtime解释的很清晰   
  ![](https://user-gold-cdn.xitu.io/2020/7/17/1735ba44fb8aaf98?imageView2/0/w/1280/h/960/format/webp/ignore-error/1 ':size=60%')

#### [详解polyfill&runtime](https://blog.liuyunzhuge.com/2019/09/04/babel%E8%AF%A6%E8%A7%A3%EF%BC%88%E4%BA%94%EF%BC%89-polyfill%E5%92%8Cruntime/)
  - https://coding.imooc.com/learn/questiondetail/gDANwYNle956K120.html
  - https://juejin.cn/post/6844904132294213639#heading-16

```js
module.exports = {
  presets: [
    '@vue/cli-plugin-babel/preset',
    {
      useBuiltIns: 'usage', // 按需引用
      corejs: 3 // polyfill
    }
  ],
  plugins: [['@babel/plugin-transform-runtime']] // 复用helper减少打包体积
}

```

#### [babel三个阶段](https://zhuanlan.zhihu.com/p/85915575)

#### [babel插件手册](https://github.com/jamiebuilds/babel-handbook/blob/master/translations/zh-Hans/plugin-handbook.md#toc-stages-of-babel)