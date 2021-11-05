---
title: unitapp 代理的二次验证
date: 2019-03-23 14:40:31
tags: js vue json
index_img: /img/unitapp.png
categories: js vue json
---

在开发unitapp  特别是app 是不需要反向代理的

跨域的本质是浏览器的拦截 但是hbuliter开发中内置浏览器 做了处理  普通浏览器安装插件也可以完成（Chrome插件名称：Allow-Control-Allow-Origin: *）

开发中配置代理测试  （这个肯定只是开发环境生效） vue.config.js

```
// vue.config.js
module.exports = {
  devServer: {
    proxy: {
      '/prefix/api/user/list': {
        target: 'https://api-remote.xxxx.com',
        pathRewrite: {
          '^/prefix': ''
        }
      }
    },
  }
}

```

另一种方式

![1636091569660](1636091569660.png)

```
"devServer" : {
            "proxy" : {
                "/prefix" : {
                    "target" : "https://www.vue-js.com",
                    "pathRewrite" : {
                        "^/prefix" : ""
                    }
                }
            }
        }
```

验证结果

![1636091607502](1636091607502.png)

![1636091622387](1636091622387.png)

这是内至浏览器

![1636091659547](1636091659547.png)

这都是谷歌

![1636091722850](1636091722850.png)

这是apk

结论只要是app 不需要加代理（vueconfig.js 和 H5 都是开发时生效）打包后没用（H5代理也是）

内置浏览器开发环境 代理不代理都可以 

谷歌浏览器需要代理才可以（插件除外）

app 不需要反向代理

开发后=H5 和线上一样  都是nginx 代理 线上部署

