---
title: vue 批量自动化
date: 2019-10-10 18:03:50
tags: element  
categories: element
index_img: img/vue.png
---



vue 批量注册全局组件 自动全局注册免去手动操作 早点下班

![1634618126400](1634618126400.png)

代码

```
// 引入vue
import Vue from 'vue'
// 引入同目录下的全部组件
const requireCom = require.context('.', false, /\.vue$/)
// 依次进行注册
requireCom.keys().forEach(key => {
  //	字符串首字母大写处理
  function strUp(str) {
    return str.charAt(0).toUpperCase() + str.slice(1)
  }
  // 获取单个组件内容
  const _component = requireCom(key)
  // 获取组件名称
  const _componentName = strUp(key.replace(/^\.\//, '').replace(/\.\w+$/, ''))

  // 注册在vue上
  Vue.component(_componentName, _component.default || _component)
})

```

在一个项目中, 某些过滤器全局都有可能用的到, 统一管理并自动化全局注册是很方便的.

代码如下, 后续只需要在**src/filters/index.js**中添加方法就可以全局使用过滤器了.

```
// src/filters/index.js

  //  格式化方法
  export function formatNull (val) {
    return val ? val : '--'
  }
```

```
// mian.js
  import Vue from 'vue'
import * as filters from './filters'


// 全局注册过滤器
Object.keys(filters).forEach(k => Vue.filter(k, filters[k]));
```

