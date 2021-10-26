---
title: plop 你值得拥有
date: 2019-11-20 15:23:24
index_img: /img/plop.png
tags: vue  plop
categories: vue  plop
---

这时代连ctrl+c  ctrl+v 都已经是工作量了
能不能简单点
再简单点

![1635239022410](1635239022410.png)

基于 Vscode 的 Snippets 自定义代码块
通过 Vscode 的 Snippets 我们可以自定义 Snippets，从而实现快捷生成代码片段。

打开 Vscode，依次点击文件——首选项——用户代码片段

```
prefix: 代码片段名字，即输入此名字就可以调用代码片段
body: 这个是代码段的主体.需要编写的代码放在这里    
$1: 生成代码后光标的初始位置
$2: 生成代码后光标的第二个位置,按 tab 键可进行快速切换,还可以有 $3,$4,$5.....
${1,字符}: 生成代码后光标的初始位置（其中 1 表示光标开始的序号，字符表示生成代码后光标会直接选中字符）
description: 代码段描述,输入名字后编辑器显示的提示信息
tab键制表符:\t
换行: \r 或者\n
```

plop 解决了静态和动态模板的问题
（常见解决方案 1代码段 比较小   2 node fs 模块写文件  3 vscode 插件 4 历史粘贴板 ）
plop 更高级

在日常开发中，我们需要不停的新建页面和组件。以 Vue 项目为例，我们在新建一个页面的时候，需要经历一遍又一遍重复的过程：

1、先新建一个文件夹

2、然后新建一个 .vue 文件，写上 
```
<template>、<script>、<style>
```

3、如果页面涉及多个组件，还要新建 component 文件夹，并重复以上两个步骤

4、最后才是我们的业务代码



开始搞

```
npm install --save-dev plop
```

- 项目根目录下新建 `plopfile.js`

```
const viewGenerator = require('./plop-templates/view/prompt')
const componentGenerator = require('./plop-templates/component/prompt')
const storeGenerator = require('./plop-templates/store/prompt.js')

module.exports = function(plop) {
  plop.setGenerator('view', viewGenerator)
  plop.setGenerator('component', componentGenerator)
  plop.setGenerator('store', storeGenerator)
}

```

index.hbs

```
{{#if template}}
<template>
  <div />
</template>
{{/if}}

{{#if script}}
<script>
export default {
  name: '{{ properCase name }}',
  props: {},
  data() {
    return {}
  },
  created() {},
  mounted() {},
  methods: {}
}
</script>
{{/if}}

{{#if style}}
<style lang="scss" scoped>

</style>
{{/if}}

```

prompt.js

```
const { notEmpty } = require('../utils.js')

module.exports = {
  description: 'generate a view',
  prompts: [{
    type: 'input',
    name: 'name',
    message: 'view name please',
    validate: notEmpty('name')
  },
  {
    type: 'checkbox',
    name: 'blocks',
    message: 'Blocks:',
    choices: [{
      name: '<template>',
      value: 'template',
      checked: true
    },
    {
      name: '<script>',
      value: 'script',
      checked: true
    },
    {
      name: 'style',
      value: 'style',
      checked: true
    }
    ],
    validate(value) {
      if (value.indexOf('script') === -1 && value.indexOf('template') === -1) {
        return 'View require at least a <script> or <template> tag.'
      }
      return true
    }
  }
  ],
  actions: data => {
    const name = '{{name}}'
    const actions = [{
      type: 'add',
      path: `src/views/${name}/index.vue`,
      templateFile: 'plop-templates/view/index.hbs',
      data: {
        name: name,
        template: data.blocks.includes('template'),
        script: data.blocks.includes('script'),
        style: data.blocks.includes('style')
      }
    }]

    return actions
  }
}

```

[政采云这个写的更不错](https://mp.weixin.qq.com/s?src=11&timestamp=1635238049&ver=3397&signature=XhxckxmQJbOV8XfbzjKEKwAUVdLu*q0A0CLP2*Hfez12F27UbOx1JtORtmjkHvZvOzswQSicEne7ipw3XXFZy11KQFltNdsHpVLfNXVgOQQwuzAy6fG5uOLpkE*Gxgdr&new=1)

![1635239420430](1635239420430.png)