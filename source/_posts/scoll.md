---
title: element table 滚动条原来这么丑
date: 2018-12-20 10:13:24
index_img: /img/gdt.png
tags: element  滚动条  
categories: element  滚动条  
---

### **element table 滚动条是可以出来 但是有两个问题**

**1样式原生太丑**
**2头部是什么鬼**

![1635228628657](1635228628657.png)

直接在app.vue 中

原生样式修改 1 滚动条   2 表格头部

```
  ::-webkit-scrollbar {
    width: 6px;
    height: 6px;
    background-color: #fff;
}
::-webkit-scrollbar-thumb {
    -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, .3);
    background-color: rgba(0, 0, 0, .1)
}
```

```
.el-table--border th.el-table__cell.gutter:last-of-type{
  background-color: rgb(242, 243, 244);
  border:0px;
} 
```

效果

![1635228755063](1635228755063.png)

![1635228779784](1635228779784.png)