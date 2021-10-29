---
title: element树形造成的坑
date: 2019-04-01 15:23:24
index_img: img/treeold.png
tags: vue element  
categories: vue element  
---

![1635495503238](1635495503238.png)

element 树形在做菜单权限逻辑的时候

1选择一个节点  递归所有父级让其选择上

2取消一个节点  递归所有子集节点让其取消

这样方便用户操作

但是

![1635495713367](1635495713367.png)

我之前也是 用  node-click 



![1635495744301](1635495744301.png)

会导致点击checkbox 不触发 node click 事件

自定义节点 加click 事件也会多次触发（无解）

![1635495799374](1635495799374.png)

最终用checked 事件就好了

核心代码

```
  enableParent(t) {
      // 启用所有父级
      const arr = this.getParentId(this.Treedata, t.menuId)
      arr.forEach(m => {
        if (!that.cloneArr.includes(m)) {
          that.cloneArr.push(m)
        }
      })
    },
    disabledChildren(t) {
      // 取消
      t.forEach(i => {
        if (that.cloneArr.includes(i.menuId)) {
          const num = that.cloneArr.indexOf(i.menuId)
          that.cloneArr.splice(num, 1)
        }
        if (i.children.length > 0) {
          that.disabledChildren(i.children)
        }
      })
    },
    customClick(data) {
      if (this.cloneArr.includes(data.menuId)) {
        // 包含就取消  所有子集取消
        that.disabledChildren([data])
      } else {
        // 不包含  启用所有父级启用
        that.enableParent(data)
      }
      this.$refs.tree.setCheckedKeys(this.cloneArr)
    },
```

```
  <el-tree
      ref="tree"
      node-key="menuId"
      :data="Treedata"
      :props="{
        children: 'children',
        label: 'menuName',
        id:'menuId'
      }"
      check-strictly
      show-checkbox
      @check="customClick"
    />
```

注意父级勾选子集不勾选

```
check-strictly
```

