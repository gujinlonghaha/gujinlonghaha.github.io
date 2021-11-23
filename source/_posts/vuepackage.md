---
title: 打包筛选无用文件和依赖
date: 2019-11-10 18:03:50
tags: vue  
categories: vue
index_img: img/db.png
---

### **如果是二次改造项目或者历史项目会出现无用文件和项目部依赖如何清理呢**

1 无用文件可以使用

```js
npm i useless-files-webpack-plugin -S
```

```js
//vue.config.js
const UselessFile = require('useless-files-webpack-plugin')
chainWebpack: config => {
    config.plugin('uselessFile')
        .use(
          new UselessFile({
            root: path.resolve(__dirname, './src'), // 项目目录
            out: './fileList.json', // 输出文件列表
            clean: false, // 是否删除文件,
            exclude: /node_modules/ // 排除文件列表
          })
        )
 
}
```

![1637656469314](1637656469314.png)

运行命令生成文件   **注意需要自己二次确认不是很准确** 但是也给人减轻了负担



2 这个主要针对依赖

![1637656578281](1637656578281.png)

```
npx depcheck
```

效果如下

![1637656640426](1637656640426.png)

自己确认后删除依赖

另外发现vue cli 默认已经开启多核能力了

![1637656697640](1637656697640.png)