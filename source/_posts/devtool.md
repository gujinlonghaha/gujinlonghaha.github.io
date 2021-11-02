---
title: 生产环境启用devtool调方法论试
date: 2020-03-13 14:27:43
index_img: /img/devtool.png
tags: vue js
categories: vue js
---



大家开发都适用vue devtool  但是线上临时调试怎办呢

![1635826701342](1635826701342.png)

自己的代码可以启用后重新打包 记得在new 之前赋值

![1635826816232](1635826816232.png)

如果是别人的代码如何热操作呢？

![1635826893932](1635826893932.png)

1 先打开网站 

f12 打开来源

ctrl+p 搜索app.js 

然后格式化

![1635826993030](1635826993030.png)

2 搜索 el: "#app"

在new 处打断点（i["default"].config.devtools=flase 默认）

![1635827098751](1635827098751.png)

3刷新页面 进入断点后 控制台输入 i["default"].config.devtools=true 回车 

跳过断点

![1635827241539](1635827241539.png)

4 f12 关掉控制台   f12  再次打开控制台 就有了vue devtool 

![1635827325490](1635827325490.png)