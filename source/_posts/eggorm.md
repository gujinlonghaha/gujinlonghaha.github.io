---
title: egg sequelize实现和自动从数据库生成model
date: 2019-06-24 15:39:41
tags: egg js
categories: egg js
index_img: /img/egg.png
---
 官网看的有点绕其实不用这么复杂

![1637222436107](1637222436107.png)

这玩意其实感觉作用不大

梳理如下

实现model 的依赖

```
npm install --save egg-sequelize mysql2
```

配置 /* egg_folder/config/config.default.js */

```
  exports.sequelize = {
    dialect: "mysql", // 数据库类型，支持 mysql，sqlite,mssql,pgsql,oracle。
    host: "127.0.0.1", // 数据库服务器地址。
    port: 3306, // 数据库连接端口号。
    database: "sys", // 数据库名称。
    username: "root", // 数据库登录用户名。
    password: "123456", // 数据库登录密码。
    define: {
      freezeTableName: true, // Model 对应的表名将与model名相同。
      timestamps: false, // 默认情况下，Sequelize会将createdAt和updatedAt的属性添加到模型中，以便您可以知道数据库条目何时进入数据库以及何时被更新
    },
    app: true, // 是否挂载app 上
  };
```

配置 /* egg_folder/config/plugin.js */

```
// 配置 egg-sequelize 插件信息。
 // 配置 egg-sequelize 插件信息。
  sequelize:{
    enable: true, // 是否启用。
    package: 'egg-sequelize', // 指定包名称。
  },
```

**自动重要生成model**  的依赖

```
npm install egg-sequelize-auto --save-dev // 对照数据库自动生成相应的 models 减少了对数据库进行增删改查时的 sql 语句的编写。
```

packjson 

```
数据库生成 models 的命令（ -o 表示生成 models 的路径，-h 表示主机，-p 表示端口，-d 表示数据库， -u 表示用户名，-x 表示密码
```

```
 "dbload": "egg-sequelize-auto -o ./app/model -h localhost -p 3344 -d database -u username -x password"   
```

执行 npm run  dbload

![1637222776962](1637222776962.png)

model 已经自动生成

启动报错 Sequelize  找不到  自己排查 原来是model

![1637223892096](1637223892096.png)

app 下找不到 

自己仿照mysql 配置成功

![1637223920935](1637223920935.png)

然后直接复制官网的改下对应model  就好了

```
"use strict";

const Controller = require("egg").Controller;
function toInt(str) {
  if (typeof str === 'number') return str;
  if (!str) return str;
  return parseInt(str, 10) || 0;
}

class SeqController extends Controller {
  async index() {
    const ctx = this.ctx;
    const query = { limit: toInt(ctx.query.limit), offset: toInt(ctx.query.offset) };
    ctx.body = await ctx.model.News.findAll(query);
  }

  async show() {
    const ctx = this.ctx;
    ctx.body = await ctx.model.News.findByPk(toInt(ctx.params.id));
  }

  async create() {
    const ctx = this.ctx;
    const { name, age } = ctx.request.body;
    const user = await ctx.model.News.create({ name, age });
    ctx.status = 201;
    ctx.body = user;
  }

  async update() {
    const ctx = this.ctx;
    const id = toInt(ctx.params.id);
    const user = await ctx.model.News.findByPk(id);
    if (!user) {
      ctx.status = 404;
      return;
    }

    const { name, age } = ctx.request.body;
    await user.update({ name, age });
    ctx.body = user;
  }

  async destroy() {
    const ctx = this.ctx;
    const id = toInt(ctx.params.id);
    const user = await ctx.model.News.findByPk(id);
    if (!user) {
      ctx.status = 404;
      return;
    }

    await user.destroy();
    ctx.status = 200;
  }
 
}
module.exports = SeqController;

```

复杂语法还得看外网地址  https://sequelize.org/master/manual/raw-queries.html