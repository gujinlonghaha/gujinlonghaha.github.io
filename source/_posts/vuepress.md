---
title: 小试尤大大vuepress vue 文档神器
date: 2020-08-01 11:15:12
index_img: /img/1618111120053.png
tags: vuepress  
categories: vuepress  
---

#  没听过vuepress？它可是新的vue全家桶成员之一，尤雨溪大神于2018年4月12日推出。 



## 个人觉得如果是vue 组件文档应该是最优选择(毕竟是vue文档工具 深度定制)

## 简洁至上

以 Markdown 为中心的项目结构，以最少的配置帮助你专注于写作。

## Vue 驱动

享受 Vue + webpack 的开发体验，可以在 Markdown 中使用 Vue 组件，又可以使用 Vue 来开发自定义主题。

## 高性能

VuePress 会为每个页面预渲染生成静态的 HTML，同时，每个页面被加载的时候，将作为 SPA 运行。

## [#](https://vuepress.vuejs.org/zh/guide/#为什么不是)为什么不是...?

### [#](https://vuepress.vuejs.org/zh/guide/#nuxt)Nuxt

VuePress 能做的事情，Nuxt 理论上确实能够胜任，但 Nuxt 是为构建应用程序而生的，而 VuePress 则专注在以内容为中心的静态网站上，同时提供了一些为技术文档定制的开箱即用的特性。

### [#](https://vuepress.vuejs.org/zh/guide/#docsify-docute)Docsify / Docute

这两个项目同样都是基于 Vue，然而它们都是完全的运行时驱动，因此对 SEO 不够友好。如果你并不关注 SEO，同时也不想安装大量依赖，它们仍然是非常好的选择！

### [#](https://vuepress.vuejs.org/zh/guide/#hexo)Hexo

Hexo 一直驱动着 Vue 的文档 —— 事实上，在把我们的主站从 Hexo 迁移到 VuePress 之前，我们可能还有很长的路要走。Hexo 最大的问题在于他的主题系统太过于静态以及过度地依赖纯字符串，而我们十分希望能够好好地利用 Vue 来处理我们的布局和交互，同时，Hexo 的 Markdown 渲染的配置也不是最灵活的。

### [#](https://vuepress.vuejs.org/zh/guide/#gitbook)GitBook

我们的子项目文档一直都在使用 GitBook。GitBook 最大的问题在于当文件很多时，每次编辑后的重新加载时间长得令人无法忍受。它的默认主题导航结构也比较有限制性，并且，主题系统也不是 Vue 驱动的。GitBook 背后的团队如今也更专注于将其打造为一个商业产品而不是开源工具。

个人小试如下

# 很快就搭建了架子 部署在了gitee 上  **配置如下**

![1618111576451](1618111576451.png)

注意配置这才会展示 官方文档有介绍  和部署域名后缀一样就ok了

![1618111629200](1618111629200.png)

测试效果如下

![1618111726401](1618111726401.png)

#### 彩蛋

codepen 如何引入到文档显得高大上？？？
如下
<iframe height="265" style="width: 100%;" scrolling="no" title="QWdaRMR" src="https://codepen.io/gujinlonghaha/embed/preview/QWdaRMR?height=265&theme-id=dark&default-tab=js,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/gujinlonghaha/pen/QWdaRMR'>QWdaRMR</a> by gujinlong
  (<a href='https://codepen.io/gujinlonghaha'>@gujinlonghaha</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>



### 步骤

先保存

![1618111961002](1618111961002.png)

然后获取iframe

![1618111992615](1618111992615.png)

最后直接放在md 源文件

![1618112040814](1618112040814.png)