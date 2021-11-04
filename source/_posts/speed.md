---
title: 记录一次单页面优化
date: 2019-08-01 09:23:24
index_img: img/speed.png
tags: vue webpack   
categories: vue webpack   
---

朋友说他的首页需要20s 才能加载完 ， 我说怎么可能？

![1636001144066](1636001144066.png)

有图有真相

![1636001165505](1636001165505.png)



![1636001212826](1636001212826.png)

开搞

1现在白屏期间增加loading 增加好感不会让人感觉漫长（自我催眠）

![1636001281138](1636001281138.png)

![1636001439190](1636001439190.png)

源码

```
 <div class="first-loading-wrp">
        <div class="loading-wrp">
          <span class="dot dot-spin"><i></i><i></i><i></i><i></i></span>
        </div>
      </div>
```

```
    <style>.first-loading-wrp{display:flex;justify-content:center;align-items:center;flex-direction:column;min-height:420px;height:100%}.first-loading-wrp>h1{font-size:128px}.first-loading-wrp .loading-wrp{padding:98px;display:flex;justify-content:center;align-items:center}.dot{animation:antRotate 1.2s infinite linear;transform:rotate(45deg);position:relative;display:inline-block;font-size:32px;width:32px;height:32px;box-sizing:border-box}.dot i{width:14px;height:14px;position:absolute;display:block;background-color:#1890ff;border-radius:100%;transform:scale(.75);transform-origin:50% 50%;opacity:.3;animation:antSpinMove 1s infinite linear alternate}.dot i:nth-child(1){top:0;left:0}.dot i:nth-child(2){top:0;right:0;-webkit-animation-delay:.4s;animation-delay:.4s}.dot i:nth-child(3){right:0;bottom:0;-webkit-animation-delay:.8s;animation-delay:.8s}.dot i:nth-child(4){bottom:0;left:0;-webkit-animation-delay:1.2s;animation-delay:1.2s}@keyframes antRotate{to{-webkit-transform:rotate(405deg);transform:rotate(405deg)}}@-webkit-keyframes antRotate{to{-webkit-transform:rotate(405deg);transform:rotate(405deg)}}@keyframes antSpinMove{to{opacity:1}}@-webkit-keyframes antSpinMove{to{opacity:1}}</style>

```

效果：

![1636001548554](1636001548554.png)

停用缓存切换3g 效果更明显

![1636001578906](1636001578906.png)

2 增加cdn  js css

```
 <% for (var i in htmlWebpackPlugin.options.cdn && htmlWebpackPlugin.options.cdn.js) { %>
      <script type="text/javascript" src="<%= htmlWebpackPlugin.options.cdn.js[i] %>"></script>
      <% } %>
```

```
 <% for (var i in htmlWebpackPlugin.options.cdn && htmlWebpackPlugin.options.cdn.css) { %>
      <link rel="stylesheet" href="<%= htmlWebpackPlugin.options.cdn.css[i] %>" />
      <% } %>
```

切记js 要在body 之前 因为打包后appjs 也会插入页面 使用vue 变量时找不到你的变量  顺序问题 

配置

```

```

![1636001770542](1636001770542.png)

```
const isProd = process.env.NODE_ENV === 'production'

const assetsCDN = {
  // webpack build externals
  externals: {
    vue: 'Vue',
    'vue-router': 'VueRouter',
    vuex: 'Vuex',
    axios: 'axios'
  },
  css: [],
  // https://unpkg.com/browse/vue@2.6.10/
  js: [
    '//cdn.jsdelivr.net/npm/vue@2.6.14/dist/vue.min.js',
    '//cdn.jsdelivr.net/npm/vue-router@3.5.1/dist/vue-router.min.js',
    '//cdn.jsdelivr.net/npm/vuex@3.1.1/dist/vuex.min.js',
    '//cdn.jsdelivr.net/npm/axios@0.21.1/dist/axios.min.js'
  ]
}
```

![1636001853392](1636001853392.png)

```
 externals: isProd ? assetsCDN.externals : {}
 
  if (isProd) {
      config.plugin('html').tap(args => {
        args[0].cdn = assetsCDN
        return args
      })
    }
```

判断是否生产环境 （本地使用依赖 线上使用cdn ）



这就大功告成了（本身页面就是动态路由  图片也压缩过  js 用压缩文件）看效果

![1636002175817](1636002175817.png)



注意 知识点

## 在项目 js 中引入

```javascript
# webpack 会检测这些组件是否在 externals 中注册
# 如果注册则不会将其打包到 app.js 中
import Vue from 'vue'
import VueRouter from 'vue-router'
import $ from 'jquery'
```

这样配置的话 webpack 在 dev 运行或 build 打包时，就不会去本地组件包中查找这些在 externals 中注册的组件了（自然也不会将他们打包到一个 app.js 中去），而是会去 window 域下直接调用 Vue, VueRouter, $ 等对象。

当然有时候也可能是你服务器的原因 可以使用ip 测速 排查

![1636002443044](1636002443044.png)

地址https://ping.chinaz.com/47.110.250.226