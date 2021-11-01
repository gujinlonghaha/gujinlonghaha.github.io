---
title: 数组循环删除引发血案 递归删除
date: 2019-02-02 16:13:24
index_img: img/arr.png
tags: js  
categories: js  
---

循环中删除元素是危险的 更别说递归删除了

![1635741289769](1635741289769.png)

```
(function () {
 var arr = [1,2,2,3,4,5];
 var len = arr.length;
 for(var i=0;i<len;i++){
 //打印数组中的情况，便于跟踪数组中数据的变化
 console.log(i+"="+arr[i]);
 //删除掉所有为2的元素
 if(arr[i]==2){
  arr.splice(i,1);
 }
 }
 console.log(arr);
})();
```



下标会出问题

我在动态菜单递归中出现此问题  解决方案（不用foeeach   使用for 循环删除时 操作i--）

```
<!-- function removeMenu(arr = [], userMenuId = []) {
  arr.forEach((t, i) => {
    if (userMenuId.includes(t.menuId) && (t.endable == 1)) {
      if (t.children && t.children.length > 0) {
        removeMenu(t.children, userMenuId)
      }
    } else {
      arr.splice(i, 1)
    }
  })
} -->
//动态下标删除问题
function removeMenu(arr = [], userMenuId = []) {
  for (let i = 0; i < arr.length; i++) {
    if (userMenuId.includes(arr[i].menuId) && (arr[i].endable == 1)) {
      if (arr[i].children && arr[i].children.length > 0) {
        removeMenu(arr[i].children, userMenuId)
      }
    } else {
      arr.splice(i, 1)
      i--
    }
  }
}
```

