---
title: vue支持可选链操作符
date: 2019-05-11 18:03:50
tags: vue js
categories: vue js
index_img: img/vue.png
---

###  在开发过程中拿到一个内嵌比较深的值需要做很多的判断，来保证没有数据而报错，比如 


```
 const obj = {
            a: {
                b: {
                    c:"1"
                }
            }
        }
取c, 正确的做法是： const cValue = (obj && obj.a && obj.a.b && obj.a.b.c) || ''; // 需要判断4次，每一层是否有值

```

 直接在链式调用的时候判断，左侧的对象是否为null或undefined。如果是的，就不再往下运算，而是返回undefined 

```
取c const cValue = obj?.a?.b?.c  //1 
```

# 如何在项目中支持可选链

```
npm install @babel/plugin-proposal-optional-chaining
```

#### 添加至项目.babel.config.js文件中：

```
{
  "plugins": [
    "@babel/plugin-proposal-optional-chaining",
 ]
}

```

全局函数

```
export default function useOptionChain(target) {

    return new Proxy(target, {
        get:  (target, propKey)=> {
            const proKeyArr = propKey.split('?.')
            return  proKeyArr.reduce((a,b)=>a?.[b],target)
        }
    })
}

```

```xml
<template>
  <div id="app">
 //保留可选链的写法，更直观的展示，想要拿数组的元素直接取下标的数字即可，不需要 []
    <h1 v-if="useOptionChain(arr)['0?.obj?.a?.b']">数组对象</h1>
    <h1 v-if="useOptionChain(obj)['arr?.0?.a']">对象数组</h1>
  </div>
</template>

<script>
//引入函数
import useOptionChain from "@/utils";

export default {
  name: "App",
  components: {},
  data() {
    return {
      arr: [
        {
          obj: {
            a: {
              b: "数组对象",
            },
          },
        },
      ],
      obj: {
        arr: [
          {
            a: "对象数组",
          },
        ],
      },
    };
  },
  methods: {
    useOptionChain,
  },
};
</script>
```