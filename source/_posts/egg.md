---
title: egg 定时任务手动开启和关闭实现(全网都很少见 )
date: 2019-06-24 15:39:41
tags: egg js
categories: egg js
index_img: /img/egg.png
---
网上很少说实现关闭的

先说理论情况 官方好像意思不管

![1637201919265](1637201919265.png)

**1 官方文档的定时任务都是开机直接运行 （之前定义好的）**

![1637202209541](1637202209541.png)

**2 我测试网友的说法也确实是这样的**

**3 继续寻找解决方案** （goolge）

![1637202275192](1637202275192.png)

 `app.schedules[key].schedule.disable = true` 



自我测试 初始值设置为 true以后还是只是执行一次

设置为false 可以实现功能 **但是定时任务不应该默认开启**

怎么办呢？

#### 自我拓展

```
  app.once('server', server => {
    
       app.schedules['F:\\study\\egg\\app\\schedule\\update_cache.js'].schedule.disable = true;
    });

```

默认定时任务开启 但是应用一启动就关闭  然后操开始关闭（注意 要运行）

```
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
```



自我总结  （全网都很少 感觉很开心）

 1 定时任务默认开启

2 应用启动后关闭

3 调用id 设置disabled  来手动开启关闭



demo效果实现 定时任务自动增加数据库数据

![1637202275192](GIF.gif)





拓展 也是也解决方案

 // 定时任务最好默认开启   可以在任务中加判断（任务一直执行 逻辑不一定要执行）

​    //1 自己也可以写定时器 数组  然后清除定时器 2可以查库字段每次判断 或者缓存(缓存关闭会消失)