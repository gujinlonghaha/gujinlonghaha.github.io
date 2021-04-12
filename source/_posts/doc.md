---
title: 文档中操作组件和展示组件源码
date: 2021-02-01 15:23:24
index_img: /img/1618210237496.png
tags: vuepress-plugin-demo-container   
categories: vuepress-plugin-demo-container   
---

## 什么是组件示例文档？

当你使用 Vue、React 或者其他语言编写了一个组件库，如 [Element UI](https://links.jianshu.com/go?to=https%3A%2F%2Felement.eleme.cn%2F2.0%2F%23%2Fzh-CN)、[Ant Design Vue](https://links.jianshu.com/go?to=https%3A%2F%2Fwww.antdv.com%2Fdocs%2Fvue%2Fintroduce-cn%2F) 或是基于它们开发的业务封装库，都需要使用文档来支撑，而使用文档必然会**包含代码与示例**，这部分就是组件示例文档。

举个例子，Element 文档，其中就包含了多份示例代码，假设我们现在完成了组件开发，需要使用 Vuepress 写一份和它一样的使用文档，该怎么做呢？

​	vuepress 官方解决方案是这样

## 导入代码段 beta

你可以通过下述的语法导入已经存在的文件中的代码段：

```md
<<< @/filepath
```

它也支持 [行高亮](https://vuepress.vuejs.org/zh/guide/markdown.html#代码块中的行高亮)：

```md
<<< @/filepath{highlightLines}
```

**输入**

```text
<<< @/../@vuepress/markdown/__tests__/fragments/snippet.js{2}
```

**输出**

```js
export default function () {
  // ..
}
```

注意

由于代码段的导入将在 webpack 编译之前执行，因此你无法使用 webpack 中的路径别名，此处的 `@` 默认值是 `process.cwd()`。

为了只导入对应部分的代码，你也可运用 [VS Code region (opens new window)](https://code.visualstudio.com/docs/editor/codebasics#_folding)。你可以在文件路径后方的 `#` 紧接着提供一个自定义的区域名称（预设为 `snippet` ）

**输入**

```md
<<< @/../@vuepress/markdown/__tests__/fragments/snippet-with-region.js#snippet{1}
```

**代码文件**

```js
// #region snippet
function foo () {
  return ({
    dest: '../../vuepress',
    locales: {
      '/': {
        lang: 'en-US',
        title: 'VuePress',
        description: 'Vue-powered Static Site Generator'
      },
      '/zh/': {
        lang: 'zh-CN',
        title: 'VuePress',
        description: 'Vue 驱动的静态网站生成器'
      }
    },
    head: [
      ['link', { rel: 'icon', href: `/logo.png` }],
      ['link', { rel: 'manifest', href: '/manifest.json' }],
      ['meta', { name: 'theme-color', content: '#3eaf7c' }],
      ['meta', { name: 'apple-mobile-web-app-capable', content: 'yes' }],
      ['meta', { name: 'apple-mobile-web-app-status-bar-style', content: 'black' }],
      ['link', { rel: 'apple-touch-icon', href: `/icons/apple-touch-icon-152x152.png` }],
      ['link', { rel: 'mask-icon', href: '/icons/safari-pinned-tab.svg', color: '#3eaf7c' }],
      ['meta', { name: 'msapplication-TileImage', content: '/icons/msapplication-icon-144x144.png' }],
      ['meta', { name: 'msapplication-TileColor', content: '#000000' }]
    ]
  })
}
// #endregion snippet

export default foo
```

**输出**

```js
function foo () {
  return ({
    dest: '../../vuepress',
    locales: {
      '/': {
        lang: 'en-US',
        title: 'VuePress',
        description: 'Vue-powered Static Site Generator'
      },
      '/zh/': {
        lang: 'zh-CN',
        title: 'VuePress',
        description: 'Vue 驱动的静态网站生成器'
      }
    },
    head: [
      ['link', { rel: 'icon', href: `/logo.png` }],
      ['link', { rel: 'manifest', href: '/manifest.json' }],
      ['meta', { name: 'theme-color', content: '#3eaf7c' }],
      ['meta', { name: 'apple-mobile-web-app-capable', content: 'yes' }],
      ['meta', { name: 'apple-mobile-web-app-status-bar-style', content: 'black' }],
      ['link', { rel: 'apple-touch-icon', href: `/icons/apple-touch-icon-152x152.png` }],
      ['link', { rel: 'mask-icon', href: '/icons/safari-pinned-tab.svg', color: '#3eaf7c' }],
      ['meta', { name: 'msapplication-TileImage', content: '/icons/msapplication-icon-144x144.png' }],
      ['meta', { name: 'msapplication-TileColor', content: '#000000' }]
    ]
  })
}
```

## 如果使用组件的话想到那个于引入一次   

组件展示一次 

源码展示一次

写两遍



有没有一次的方法呢？（意外发现）

工具总是提高人们效率的法宝之一

使用 `npm` 安装它：

```bash
npm i vuepress-plugin-demo-container --save-dev
```

如果你的网络环境不佳，推荐使用 [cnpm](https://github.com/cnpm/cnpm)。

### [#](https://calebman.github.io/vuepress-plugin-demo-container/zh/started.html#配置插件)配置插件

打开 .vuepress/config.js 文件，然后在合适的位置引用插件：

```js
module.exports = {
  ...
  plugins: ['demo-container']
  ...
}
```

在 Markdown 文件中编写以下代码：

~~~html
::: demo 此处放置代码示例的描述信息，支持 `Markdown` 语法，**描述信息只支持单行**
```html
<template>
  <div class="red-center-text">
      <p>{{ message }}</p>
      <input v-model="message" placeholder="Input something..."/>
  </div>
</template>
<script>
export default {
  data() {
    return {
      message: 'Hello Vue'
    }
  }
}
</script>
<style>
.red-center-text { 
  color: #ff7875;
  text-align: center;
}
</style>
` ` `  <= 删除左侧空格
:::
~~~

运行效果如下

![1618210827882](1618210827882.png)

[个人demo 地址](http://gujinlong_705.gitee.io/vuepresstest/)

