---
title: redis mongdb 安装和操作 egg 
date: 2019-06-25 15:39:41
tags: egg js
categories: egg js
index_img: /img/sjk.png
---
![1637312809603](1637312809603.png)

redis 下载安装地址

![1637312827039](1637312827039.png)

 **https://github.com/tporadowski/releases。** 

安装时最后勾选环境变量

进入他的安装目录

```
C:\Program Files\Redis
```

执行命令 

```
redis-cli.exe -h 127.0.0.1 -p 6379
```

设置数据 和获取数据效果如下

![1637312179874](1637312179874.png)

下面是 egg 操作

```
npm i egg-redis --save
```

```
//Change ${app_root}/config/plugin.js to enable redis plugin:

exports.redis = {
  enable: true,
  package: 'egg-redis',
};
```

```
config.redis = {
  client: {
    port: 6379,          // Redis port
    host: '127.0.0.1',   // Redis host
    password: 'null', // 没有密码也得写null
    db: 0, //默认心苦都是0
  },
}
```

控制器语法

```
  async get() {
    const { ctx,app } = this;
    ctx.logger.warn(ctx.query); //有
    const list = await app.redis.get('foo');
    ctx.body = list;
  }
  async post() {
    const { ctx,app } = this;
    const list = await app.redis.set('foo', 'bar')
    ctx.body = list;
  }
```

就大功告成了

mongdb

下载地址

```
https://www.mongodb.com/try/download/community
```

![1637312442087](1637312442087.png)

一直下一步安装完成

进入安装目录

```
C:\Program Files\MongoDB\Server\5.0\data
```

默认没有数据db 文件

新建文件夹db

![1637312606618](1637312606618.png)

双击目录下exe 就可以下面操作了

![1637312681198](1637312681198.png)

egg 操作

```
npm i egg-mongodb --save
```

```
  exports.mongodb = {
        enable: true,
        // feel free to make some local change, and require it like this.
        // path: your_local_folder_path
        package: 'egg-mongodb'
    };
```

```
 config.mongodb = {
            app: true,
            agent: false,
            username: '',
            password: '',
            hosts: '127.0.0.1:27017',
            db: 'test',
            query: '',
            // defalut: {
            //     username: '',
            //     password: '',
            //     hosts: '127.0.0.1:27017',
            //     db: 'test',
            //     query: ''
            // },
            // client: {
            //     username: '',
            //     password: '',
            //     hosts: '127.0.0.1:27017',
            //     db: 'test',
            //     query: ''
            // }
        };
```

```
 async getmongdb() {
    const { ctx,app } = this;
    let db = this.app.mongodb;
    let User = db.collection('user');
    const list = await User.insertOne({
      name: 'zhang san',
      phone: '177xxxxxxxx'
  });
    ctx.body = list;
  }
  async postmongdb() {
    const { ctx,app } = this;
    let db = this.app.mongodb;
    let User = db.collection('user');
    const list = await User.findOne({
      name: 'zhang san'
  });
    ctx.body = list;
  }
```

基本搭建就完成了

redis mongdb 命令地址 
https://www.runoob.com/redis/redis-tutorial.html
https://www.runoob.com/mongodb/mongodb-tutorial.html