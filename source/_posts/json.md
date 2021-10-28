---
title: json在线编辑器在字典很适合
date: 2020-04-23 10:40:31
tags: js vue json
index_img: /img/json.png
categories: js vue json
---

### 有时候字典的业务场景手动录入很麻烦  我们直接当做JSON 字符串来处理好处

**很快录入 不需要1个1个点击页面添加**

**坏处 之前有人录入搞进去了/r /n  换车 换行导致 解析错误 json.parse()**



![1635302321909](1635302321909.png)

原来是这样的   很粗暴

![1635302347871](1635302347871.png)

现在增加了 json 编辑器 就可以校验json 了

![1635302734394](1635302734394.png)

用法

```
npm install vue-json-editor --save
```

官方文档不全

![1635302806720](1635302806720.png)

真正的配置大全

```
<template>
  <div>
    <p>vue-json-editor</p>
    <vue-json-editor
    v-model="json" 
    :show-btns="true"  // 是否显示保存按钮
    :mode="'code'" // 默认编辑模式
    lang="zh"  // 显示中文，默认英文
    @json-change="onJsonChange"  // 数据改变事件
    @json-save="onJsonSave" 
    @has-error="onError"></vue-json-editor>
  </div>
</template>
 
<script>
  import vueJsonEditor from 'vue-json-editor'
 
  export default {
    data () {
      return {
        json: {
          msg: 'demo of jsoneditor'
        }
      }
    },
 
    components: {
      vueJsonEditor
    },
 
    methods: {
      onJsonChange (value) {
        // 异常时不会打印
            console.log('value:', value);
        },
      onJsonSave (value) {
            console.log('value:', value);
        },
       onError (value) {
           
            console.log('value:', value);
        }
    }
  }
</script>
```

```
model：bind the [json object] 
:show-btns: boolean, show the save button, default: true 
:expandedOnStart: boolean, expand the JSON editor on start for the modes 'tree', 'view', and 'form', default: false 
:mode: string, default: tree 
:lang: string, default:en
 @json-change: on json changed
@json-save: on json save
@has-error: on error
```

