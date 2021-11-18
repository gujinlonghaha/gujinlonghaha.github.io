---
title: egg集成swagger
date: 2019-06-24 17:30:41
tags: egg js
categories: egg js
index_img: /img/swagger.png
---
传统老牌接口调试都是swagger 

egg 集成 swagger  其实还是挺简单的

https://github.com/Yanshijie-EL/egg-swagger-doc

安装插件

```
$ npm i egg-swagger-doc --save
```

配置

```
// {app_root}/config/plugin.js
exports.swaggerdoc = {
  enable: true,
  package: 'egg-swagger-doc',
};
```

定制配置

```
// {app_root}/config/config.default.js
exports.swaggerdoc = {
  dirScanner: './app/controller',
  apiInfo: {
    title: 'egg-swagger',
    description: 'swagger-ui for egg',
    version: '1.0.0',
  },
  schemes: ['http', 'https'],
  consumes: ['application/json'],
  produces: ['application/json'],
  securityDefinitions: {
    // apikey: {
    //   type: 'apiKey',
    //   name: 'clientkey',
    //   in: 'header',
    // },
    // oauth2: {
    //   type: 'oauth2',
    //   tokenUrl: 'http://petstore.swagger.io/oauth/dialog',
    //   flow: 'password',
    //   scopes: {
    //     'write:access_token': 'write access_token',
    //     'read:access_token': 'read access_token',
    //   },
    // },
  },
  enableSecurity: false,
  // enableValidate: true,
  routerMap: false,
  enable: true,
};

```

注意必须配置文件

```
/* egg_folder/app/contract/format.js */
const JsonBody = {
  code: { type: 'number', required: true, example: 0 },
  message: { type: 'string', required: true, example: 'success' },
  data: { type: 'Enum', required: true, example: [] },
}
module.exports = {
  indexJsonBody: {
    ...JsonBody,
    data: { type: 'string', example: 'test' },
  },
};
```

页面配置

![1637220105068](1637220105068.png)

本地启动测试地址 http://127.0.0.1:7002/swagger-ui.html

在线测试都是可以的 

![1637220150307](1637220150307.png)

编写文档还是有些麻烦  不过可以使用apifox 或者apipost 更简单些 