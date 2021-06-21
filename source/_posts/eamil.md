---
title: 亲手搭建node自动发送邮件和祝福语
index_img: /img/1619757995805.png
date: 2021-05-20 10:23:24
tags: nodemailer eamil qq
categories: nodemailer eamil qq
---

### **适合场景：node 自动发送邮件  祝福语 日志 授权码  大家可以随意把玩**

搭建环境

```
node init -y
npm  install  node-schedule axios nodemailer -D
```

新建 index.js

```js
var nodemailer = require('nodemailer');
const axios = require('axios');
const schedule = require("node-schedule");

var transporter = nodemailer.createTransport({
  host: 'smtp.qq.com',
  auth: {
    user: '自己邮箱',
    pass: '自己邮箱授权码' //授权码,通过QQ获取
 
  }
  });
  var mailOptions = {
    from: '', // 发送者
    to: '', // 接受者,可以同时发送多个,以逗号隔开
    subject: '邮件发送', // 标题
    text: 'Hello world', // 文本
    html: `<h2>nodemailer基本测试:</h2><h3>gjl测试` 
  };
 

  //   网上开放api   https://api.muxiaoguo.cn/doc/caihongpi.html 
  function getUserAccount() {
    //   随机文字
    return axios.get('https://api.muxiaoguo.cn/api/caihongpi');
  }
  
  function getUserPermissions() {
    //   随机图片  https://api.loli.bj/
    return axios.get('https://api.loli.bj/api/?type=json');
  }
  



function init(){
    axios.all([getUserAccount(), getUserPermissions()])
    .then(axios.spread(function (response, imgpon) {
        console.log(response?.data?.data?.comment);
        console.log(imgpon?.data?.url);
        // 文本
        mailOptions.html=response?.data?.data?.comment    
        // 图片
        mailOptions.html=  mailOptions.html + '<img src="cid:ingurl"/>'
        mailOptions.attachments=[{
            filename: 'image.png',
            path: imgpon?.data?.url,
            cid: 'ingurl' //same cid value as in the html img src
        },
        {   // filename and content type is derived from path
            path: './file.txt'
        },
    ]
        // 发送邮件
          transporter.sendMail(mailOptions, function (err, info) {
          if (err) {
            console.log(err);
            return;
          }
          console.log('发送成功');
        });
      // 两个请求现在都执行完成
    })).catch(function (error) {
        console.log("接口异常");
      });;

}

// 每格一分钟发送邮件
const job = schedule.scheduleJob(' */1 * * * *', function(){
    console.log('The answer to life, the universe, and everything!');
    init()
  });

// 单词发送
//   init()


```

效果如下

![1619759161403](1619758351206.png)

![1619759161403](1619758370409.png)

faq :授权码获得

 ![1619759161403](1619758513964.png) 



部分想要拓展的配置 ![1619759161403](1619758573724.png) 