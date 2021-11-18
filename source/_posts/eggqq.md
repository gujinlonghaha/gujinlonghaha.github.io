---
title: 自己搭建服务端egg请求那些事
date: 2019-06-23 16:39:41
tags: egg js
categories: egg js
index_img: /img/egg.png
---
自己安装window mysql 和取款机一样next 
新建表 右键设计表 主键设置自增 这样post 新增请求就不用写唯一值了

mysql8 navcat 连接不上

ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'password'
flush privileges
执行这个就好了

当你手动还在增删改的时候官方很多都帮你集成了 这是我自我实现的对比

![1637203801807](1637203801807.png)

官方一行确实很给力

![1637203881752](1637203881752.png)



1修改和删除需要在地址上传id

2 官方文档别直接copy 函数应该是  如果两个单词官方建议下划线分割

```
"use strict";

const Controller = require("egg").Controller;
const path = require("path");
const fs = require("fs");

class TestController extends Controller {
  async index() {
    const { ctx } = this;
    const list = await ctx.service.add.get(ctx.query);
    ctx.body = ctx.helper.initData(list);
  }
  async create() {
    const { ctx } = this;
    const list = await ctx.service.add.post(ctx.request.body);
    ctx.body = ctx.helper.initData(list);
  }
  async start() {
    const { ctx,app } = this;
    app.schedules['F:\\study\\egg\\app\\schedule\\update_cache.js'].schedule.disable = false
    console.log('我是开启')
    console.log(app.schedules['F:\\study\\egg\\app\\schedule\\update_cache.js'].schedule.disable);
    // await app.runSchedule('update_cache');
    ctx.body = ctx.helper.initData("成功");
  }
  async stop() {
    const { ctx,app } = this;
    app.schedules['F:\\study\\egg\\app\\schedule\\update_cache.js'].schedule.disable = true
    console.log(app.schedules['F:\\study\\egg\\app\\schedule\\update_cache.js'].schedule.disable);
    console.log('我是关闭')
    ctx.body = ctx.helper.initData("成功");
  }
  async update() {
    const { ctx } = this;
    const list = await ctx.service.add.put(ctx.request.body);
    ctx.body = ctx.helper.initData(list);
  }
  async destroy() {
    const { ctx } = this;
    const list = await ctx.service.add.delete(ctx.request.body);
    ctx.body = ctx.helper.initData(list);
  }
  async postTest() {
    const { ctx } = this;
    // ctx.logger.warn(ctx.request.body); //有
    const list = await ctx.service.add.post(ctx.request.body);
    ctx.body = ctx.helper.initData(list);
  }
  async setToken() {
    const { ctx, app } = this;
    const token = app.jwt.sign(
      { username: "username", }, 
      app.config.jwt.secret
    );
    ctx.cookies.set( 'authorization', token ); 
    ctx.body = ctx.helper.initData(token);
  }
  async getToken() {
    const { ctx} = this;
    let token=ctx.cookies.get('authorization' ); 
    ctx.body = ctx.helper.initData(token);
  }
  async upload() {
    const { ctx } = this;
    ctx.logger.warn(ctx.request.files[0]); //有
    // const list = await ctx.service.add.post(ctx.request.body);
    let file = ctx.request.files[0];
    let filepath = fs.readFileSync(file.filepath); //files[0]表示获取第一个文件，若前端上传多个文件则可以遍历这个数组对象
    // 将文件存到指定位置  静态文件随机数和后缀
    let name = `${Math.ceil(Math.random() * 100)}${path.extname(
      file.filename
    )}`;
    fs.writeFileSync(path.resolve(this.app.config.static.dir, name), filepath);
    ctx.body = { name };
  }
  // 下载本地文件
  async download() {
    const filePath = path.resolve(this.app.config.static.dir, "gjl.txt");
    this.ctx.attachment("gjl.txt");
    this.ctx.set("Content-Type", "application/octet-stream");
    this.ctx.body = fs.createReadStream(filePath);
  }
  //  下载远端图片
  async downloadImage() {
    const url = "http://cdn2.ettoday.net/images/1200/1200526.jpg";
    const res = await this.ctx.curl(url, {
      streaming: true,
    });

    this.ctx.type = "jpg";
    this.ctx.body = res.res;
  }
}
module.exports = TestController;
```

3 没写的不注册还是很人性化的

5 实测除了get 请求 put post delete 请求body params 都能传 服务端可接收

post json 和 post form （无文件 ）egg 获取参数无差异 前端需要（axios）

```
 method: 'post',
          headers: { 'content-type': 'application/x-www-form-urlencoded' },
          url: '/mock/postTest',
          data: qs.stringify(this.form)
```

如果包含文件   请求数据格式就必须以 [multipart/form-data](http://tools.ietf.org/html/rfc2388) 进行提交了。 

egg 服务端需要配置 不是流的格式  接收直接看上面代码 upload 
取值ctx.request.files
普通字段都在ctx.request.body 上

```
  config.multipart = {
    mode: 'file'
  };
```

可以下载本地文件 也可以远端 直接看demo



![1637204669533](1637204669533.png)

阮一峰 接口协议介绍  http://www.ruanyifeng.com/blog/2014/05/restful_api.html

6 token 可以设置cookie 自动带回来但是 如果客户端不是浏览器就凉了 还是客户端每次设置到请求头靠谱

插件 egg-jwt 使用直接看文档 

7 自我功能实现汇总展示

![1637204871568](1637204871568.png)

8 统一返回值 直接写个helper 函数包装一下

4 get 没事  post 和put  cors  开启下面或者自己配置

```
  config.security = {
    csrf: {
      enable: false,
      ignoreJSON: true
    },
    // 允许访问接口的白名单
    domainWhiteList: ['*'] // ['http://localhost:8080']
  }

```

