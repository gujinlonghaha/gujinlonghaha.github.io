---
title: 记一次抢购九价历程
date: 2022-01-10 17:59:41
tags: 抢购
categories: 抢购
index_img: /img/qg.png
---

最近九价疫苗很多堪比疫苗界的茅台，溢价几百到上千，因为刚需导致很多去国外接种

操作相对简单

![1641951355022](1641951355022.png)

基本就是点击预约和提交订单，我使用了时间悬浮和自动化点击操作，延时 2ms ,但是十来万人抢购还是失败，

![1641951489626](1641951489626.png)

然后去排查优化措施，进行 FIDDLER 小程序抓包

发现域名是![1641951589542](1641951589542.png)

进行域名反向 i 查询

https://seo.chinaz.com/miaomiao.scmttec.com

![1641951650718](1641951650718.png)

进行 ip 详情

https://ip.tool.chinaz.com/?ip=159.75.189.235

![1641951791154](1641951791154.png)

发现是腾讯云设备 在广州市

接下来进行 ip 全国测速

http://ping.chinaz.com/159.75.189.235

![1641951908132](1641951908132.png)

不出所料 延迟做少是广东 平均延时 61.ms

实测本机延时

![1641951986838](1641951986838.png)

基本在 50ms

也就是说点击延时 2ms 但是网络延时 50ms 实际延时在 52ms 左右

怪不得那些人都要用云服务器 这样就可以甩别人一条街
