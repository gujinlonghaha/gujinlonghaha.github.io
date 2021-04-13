---
title: 文档中代码编辑加预览
date: 2020-10-13 14:27:43
index_img: /img/1618295601686.png
tags: docsify vuepres code run
categories: docsify vuepres code run
---



## 好文档是什么呢？（其实有人已经回答）

我一直在思考 🤔 怎么的 Vue 文档交互才是好的 👍, 后来得出的结论是:

- 能看代码
- 能看效果
- 能在线编辑代码, 并实时预览结果

能做到前 2 点的 Vue 组件不少, 但能做到第 3 点, 并对文档的 DEMO 编写做优化处理的并不多



找了好久功夫不负有心人  还是有大神造就出了轮子   轮子总是能效率提升百倍

以 docsify 为例子     （v-charts  展示源码同时 可预览 编辑  就是用的这个做的文档 虽然线上目前出故障了到是 ）  [地址](https://v-charts.js.org/#/line)

原理使用vuep  这是个好东西

![1618296154343](1618296154343.png)





随后又找到另一个类似网站  [ve-charts](https://vueblocks.github.io/ve-charts/#/chart-pie)

1. 有源码  
2. 有demo
3. 源码编辑 demo 联动  

![1618296377359](1618296377359.png)



以上都是vuep 技术实现

### 还没有没别的方式呢    

#### **vue-run-sfc**  你值得拥有  

## 🍎 使用

```
<script>
  window.$docsify = {
    // 配置, 更多属性解释请往下面翻 ↓
    run: {
      themeColor: 'green',
      themeBorderColor: '#eee',
      reverse: true,
      // ...
    }
  }
</script>

<!-- 引入Vue -->
<script src="//unpkg.com/vue/dist/vue.min.js"></script>
<!-- 引入 vue-run-sfc -->
<script src="//unpkg.com/vue-run-sfc"></script>
<!-- 引入 docsify-plugin-run -->
<script src="https://unpkg.com/docsify-plugin-run/src/index.js"></script>
<!-- 指定版本 -->
<!-- <script src="https://unpkg.com/docsify-plugin-run@xx.xx.xx/src/index.js"></script> -->
<!-- docsify -->
<script src="//unpkg.com/docsify/lib/docsify.min.js"></script>
```

## 🍊示例

### 全局配置: 运行 element-ui

```
<script>
  window.$docsify = {
    run: {
      jsLabs: ['https://unpkg.com/element-ui/lib/index.js'],
      cssLabs: ['https://unpkg.com/element-ui/lib/theme-chalk/index.css'],
    }
  }
</script>
``html run
<template>
  <el-tabs type="border-card">
    <el-tab-pane label="用户管理">用户管理</el-tab-pane>
    <el-tab-pane label="配置管理">配置管理</el-tab-pane>
    <el-tab-pane label="角色管理">角色管理</el-tab-pane>
    <el-tab-pane label="定时任务补偿">定时任务补偿</el-tab-pane>
  </el-tabs>
</template>
``  <== 这里和上面的 ` 有 3 个
```



### 最终效果我放在了[码云地址](http://gujinlong_705.gitee.io/docsifytest/#/test)

简单测试效果如图 

![1618296592012](1618296592012.png)