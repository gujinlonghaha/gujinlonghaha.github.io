---
title: 帮运维快速切换内外网(二网双选一) 另附bat 转exe
date: 2021-04-12 15:39:41
tags: 网卡 bat
categories: 网卡 bat
index_img: /img/1618386156421.png
---

## 运维同事因为客户环境而发愁

1. 因为客户(政府 电信等)环境是内网，
2. 平时通信查问题是外网
3. 来回切换网线 切换网卡 

我想在网上检索个工具应该能解决吧？ 结果却不尽人意  没有合适的工具

![1618388010418](1618388010418.png)

不是根本不起作用就是只能切换ip 难道没有办法了吗？



### **最终自己编写bat 解决 完美撒花**

汉字 专网  WLAN 分别是是网卡对应名称  记得修改

```bat

@echo off
title gjl切换网络脚本
:stillAnswer
set /p switch=1切换为专网，2切换为WLAN，3切换为双网卡，4退出：
if %switch% == 1 (
netsh interface set interface name="专网" admin=enabled
netsh interface set interface name="WLAN" admin=disabled

echo -------------------当前网络为专网---------------------
goto stillAnswer
)else if %switch% == 2 (
netsh interface set interface name="专网" admin=disabled
netsh interface set interface name="WLAN" admin=enabled
echo -------------------当前网络为WLAN---------------------
goto stillAnswer
)else if %switch% == 3 (
netsh interface set interface name="专网" admin=enabled
netsh interface set interface name="WLAN" admin=enabled
echo -------------------双网卡同时启动---------------------
goto stillAnswer
)
else if %switch% == 4 (
exit
)

::netsh interface set interface name="WLAN" admin=disabled
::netsh interface set interface name="专网" admin=enabled

::netsh interface set interface name="WLAN" admin=enabled
::netsh interface set interface name="专网" admin=disabled
```

新建bat文件保存后**管理员身份**运行 效果如下：

![1618388625946](1618388625946.png)



这样还不够完美 我有用bat 转为exe 

![1618388732373](1618388732373.png)

如何bat转exe?   工具我上传了

下载:https://wws.lanzous.com/iVXwko1hiqb 密码:9qtj



我也发布到了quicker

![1618389340230](1618389340230.png)