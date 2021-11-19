---
title: 梳理极简egg socket 
date: 2019-06-25 15:39:41
tags: egg js
categories: egg js
index_img: /img/egg.png
---
egg 上的socket 包含了redis和房间 功能复杂 乍一看还是有点绕的

我梳理了极其简单demo 

自己实测梳理逻辑如下

```
$ npm i egg-socket.io --save
```

```
// {app_root}/config/plugin.js
exports.io = {
  enable: true,
  package: 'egg-socket.io'，
};
```
```
// {app_root}/package.json
{
  "scripts": {
    "dev": "egg-bin dev --sticky",
    "start": "egg-scripts start --sticky"
  }
}
```

配置命名空间

```
// {app_root}/config/config.${env}.js
exports.io = {
  init: { }, // passed to engine.io
  namespace: {
    '/': {  //对应router.js里的 of('/')
        connectionMiddleware: ['auth'], //对应io/middleware/auth
        packetMiddleware: ['back'], //对应io/middleware/back
  },
};
```

在app 下面新建自己写的 文件  也就是控制器和中间件    路由的话还走系统路由就行

![1637305985050](1637305985050.png)

```
// chart.js
"use strict";

const Controller = require("egg").Controller;
class ChartController extends Controller {
   async chat() {
      const { ctx, app } = this;
       //方法通过 客户端  socket.emit("chat", {a:'我地址chat是测试信息'});
      const params = this.ctx.args;
      console.log('客户端消息', params);
      this.ctx.socket.emit('chat', params);
   }
   async mess() { 
      //方法通过 客户端  socket.emit("mess", {a:'我地址chat是测试信息'});
      const params = this.ctx.args;
      console.log('客户端消息', params);
      this.ctx.socket.emit('message', params);
   }
}
```

```
// websocket  这里是路由 
  io.of('/').route('chat', io.controller.chart.chat);
  io.of('/').route('mess', io.controller.chart.mess);
```

在页面新建html

```
<!DOCTYPE html>
<html lang="en">

<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Demo</title>
  <style>
    body {
      overflow-x: hidden;
    }
    .console-wrapper {
      margin: auto;
      padding: 12px;
      width: 80%;
      background: #eee;
    }
  </style>
</head>

<body>
  <div class="console-wrapper">
    <pre id="console"></pre>
  </div>
  <script src="https://cdn.bootcss.com/socket.io/2.1.0/socket.io.js"></script>
  <script src="https://cdn.bootcss.com/lodash.js/4.17.10/lodash.min.js"></script>
  <script>
    // 不重要的代码，仅展示使用 - start
    const con = document.querySelector('#console');
    const doc = document.documentElement;
    const wh = document.documentElement.clientHeight;

    const _scrollToBottom = (function() {
      return _.throttle(function() {
        doc.scrollTop = doc.scrollHeight;
      }, 100); 
    })();

    const scrollToBottom = function() {
      if (doc.scrollHeight > wh) {
        _scrollToBottom();
      }
    };

    const log = function() {
      let msgList = [].slice.apply(arguments);
      msgList = msgList.map(function(msg) {
        if (typeof msg !== 'object') {
          return msg;
        }
        try {
          return JSON.stringify(msg, null, 2);
        } catch(error) {
          return _.toString(msg);
        }
      });
      con.innerText += new Date().toLocaleString() + ' ' + msgList.join('') + '\n';
      scrollToBottom();
      console.log.apply(null, arguments);
    };

    // 不重要的代码，仅展示使用 - end

    window.onload = function () {
      // init
     
      const socket = io('ws://localhost:7002/', {

        // 实际使用中可以在这里传递参数
        query: {
          room: 'demo',
          userId: `client_${Math.random()}`,
        },
        transports: ['websocket']
      });

      socket.on('connect', () => {
        const id = socket.id;
        socket.send("我是客户端Hello!");

        log('#connect,', id, socket,"已经建立连接");
    
        socket.emit("mess", {a:'我地址mess是测试信息'});
        setInterval(() => {
            socket.emit("chat", {a:'我地址chat是测试信息'});
        }, 5000);
      });
      socket.on("message", msg => {
        log('我是服务端消息:', msg);
      });

      socket.on("chat", (msg) => {
        log('我是服务端chat消息:', msg);
      })
      // 系统事件
      socket.on('disconnect', msg => {
        log('#disconnect', msg);
      });

      socket.on('disconnecting', () => {
        log('#disconnecting');
      });

      socket.on('error', () => {
        log('#error');
      });

      window.socket = socket;
    };

  </script>
</body>

</html>
```

效果如下

![1637306423622](1637306423622.png)

总结

这里涉及socket 知识 需要参考官方

https://socket.io/docs/v4/

html 模板也可以参照地址开发

https://github1s.com/eggjs-community/demo-egg-socket.io/blob/HEAD/app/view/home.html#L82-L83

socket  客户端 监听回掉事件 注意事项

比如 服务端路由是

```
  io.of('/').route('chat', io.controller.chart.chat);
```

客户端监听chart响应事件

```
  socket.on("chat", (msg) => {
        log('我是服务端chat消息:', msg);
      })
```

