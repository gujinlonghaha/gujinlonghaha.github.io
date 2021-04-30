---
title: 小记：网页调用app 启动并打开固定地址
date: 2021-03-30 16:46:15
index_img: /img/1619772522632.png
tags: app
categories: app
---

我们能在浏览器内跳转网页是http、https协议，能打开本地文件是file协议，调用APP用的是scheme协议或者intent协议，其实都是发送请求，获得响应，只是网页和APP是使用这两个协议通信的。scheme协议不仅仅用于ios端，也适用于安卓。但intent只适用于安卓。

## 1、scheme协议

在 iOS 里，程序之间都是相互隔离，目前并没有一个有效的方式来做程序间通信，幸好 iOS 程序可以很方便的注册自己的 URL Scheme，这样就可以通过打开特定 URL 的方式来传递参数给另外一个程序。原生的APP和网页通过这种协议可以互相通信，safari或者APP的webView都可以识别该协议（一些浏览器如微信内置浏览器和QQ浏览器不支持，据后台技术哥哥介绍这是腾讯有意屏蔽，默认的webview控件是支持的）.

如果是自己的APP想被网页调用，需要自己在app中注册scheme，这不是我研究的范畴。。。

我们一般是调用别人的APP，会使用scheme协议就行,其实特简单，**通过链接就可以**。

### 常用APP的scheme

QQ mqq://
微信 weixin://
淘宝 taobao://
大众点评 dianping:// dianping://search
新浪微博 sinaweibo://
支付宝 alipay://
豆瓣fm doubanradio://
美团 imeituan://
1号店 wccbyihaodian://
有道词典yddictproapp://
知乎 zhihu://
优酷 youku://

见知乎上的整理：http://www.zhihu.com/question/19907735

## 2、intent协议

“在一个Android应用中，主要是由四种组件组成的，这四种组件可参考“Android应用的构成”。而这四种组件是独立的，它们之间可以互相调用，协调工作，最终组成一个真正的Android应用。在这些组件之间的通讯中，主要是由Intent协助完成的。Intent负责对应用中一次操作的动作、动作涉及数据、附加数据进行描述，Android则根据此Intent的描述，负责找到对应的组件，将Intent传递给调用的组件，并完成组件的调用。因此，Intent在这里起着一个媒体中介的作用，专门提供组件互相调用的相关信息，实现调用者与被调用者之间的解耦。”所以说intent是协议是不准确的，intent实际上是一种在不同组件之间传递的请求信息。

# 示例：网页调取百度地图、高德地图

官方文档：

1、高德地图：http://lbs.amap.com/api/uri-api/ios-uri-explain/

2、百度地图：http://developer.baidu.com/map/index.php?title=uri/api/ios

测试：

```
<a href="taobao://detail.m.tmall.com/item.htm?spm=a2141.8971817.50003458.1&id=612053462785&scm=1007.15522.117270.cat:50003458_industry:_cattype:cat_id:612053462785&pvid=0982d09f-d63f-438f-af91-5434a11305f4&scene=5522">大萨达撒</a>
```

手机端调用页面 

页面地址获取

![1619772927650](1619772927650.png)