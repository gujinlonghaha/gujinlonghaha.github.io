---
title: 修改你的ntp服务器
date: 2021-04-16 14:45:46
index_img: /img/1618556161785.png
tags: ntp 
categories: ntp
---

##  **一、NTP 是什么？**

网络时间协议，英文名称：Network Time Protocol（NTP）是用来使计算机[时间同步](https://baike.baidu.com/item/时间同步)化的一种协议，它可以使[计算机](https://baike.baidu.com/item/计算机/140338)对其[服务器](https://baike.baidu.com/item/服务器/100571)或[时钟源](https://baike.baidu.com/item/时钟源/3219811)（如石英钟，GPS等等)做同步化，它可以提供高精准度的时间校正（LAN上与标准间差小于1毫秒，WAN上几十毫秒），且可介由加密确认的方式来防止恶毒的[协议](https://baike.baidu.com/item/协议/670528)攻击。NTP的目的是在无序的Internet环境中提供精确和健壮的时间服务。 

## 二、NTP 实现什么目的？

目的很简单，就是为了提供准确时间。因为我们的手表、手机、电脑等设备，经常会跑着跑着时间就出现了误差，或快或慢的少几秒，时间长了甚至误差过分钟。

## 三、NTP 服务器列表

Windows系统上自带的两个：`time.windows.com` 和 `time.nist.gov`
MacOS上自带的两个：`time.apple.com` 和 `time.asia.apple.com`
NTP授时快速域名服务：`cn.ntp.org.cn`

但是上面的基本上同步起来很慢，甚至直接同步超时，毕竟国外的服务器。所以推荐大家用国内的服务器同步。

### 1、阿里云授时服务器



```css
#NTP服务器
ntp.aliyun.com             
ntp1.aliyun.com
ntp2.aliyun.com
ntp3.aliyun.com
ntp4.aliyun.com
ntp5.aliyun.com
ntp6.aliyun.com
ntp7.aliyun.com

#Time服务器
time1.aliyun.com
time2.aliyun.com
time3.aliyun.com
time4.aliyun.com
time5.aliyun.com
time6.aliyun.com
time7.aliyun.com
```

### 2、国内大学授时服务器



```css
s1c.time.edu.cn       北京大学 
s2m.time.edu.cn       北京大学
s1b.time.edu.cn       清华大学
s1e.time.edu.cn       清华大学
s2a.time.edu.cn       清华大学
s2b.time.edu.cn       清华大学
```

### 3、国外授时服务器



```css
#苹果提供的授时服务器   
time1.apple.com
time2.apple.com
time3.apple.com
time4.apple.com
time5.apple.com
time6.apple.com
time7.apple.com

#Google提供的授时服务器   
time1.google.com
time2.google.com
time3.google.com
```

[**window修改ntp 方法 参考百度经验**](https://jingyan.baidu.com/article/4dc40848556be9c8d846f17d.html)

或者工具直接修 自行搜索

![1618556011733](1618556011733.png)

需要同步系统时间，这里分享两个时间服务器接口api给大家：

1.淘宝时间服务器时间接口

http://api.m.taobao.com/rest/api3.do?api=mtop.common.getTimestamp

返回json数据

```
{"api":"mtop.common.getTimestamp","v":"*","ret":["SUCCESS::接口调用成功"],"data":{"t":"1586519130440"}}
```

2.苏宁时间服务器接口api：

http://quan.suning.com/getSysTime.do

返回json数据

```
{"sysTime2":"2020-04-10 19:46:50","sysTime1":"20200410194650"}
```



demo： 自己修改如下

![1618556100576](1618556100576.png)