---
title: 搭建自己的图床
date: 2021-04-9 16:53:55
tags: 图床 图片 gitee
index_img: /img/1618304835171.png

---


## 1. 什么是图床

简单来说就是存储图片的服务器，将图片上传至该服务器中后，可以在公网中通过指定的URL获取此图片。

## 2. 图床的意义

- 减轻服务器带宽压力：图片资源可以由单独的服务器来存储、访问，例如腾讯云的COS、阿里云的OSS、七牛云等产品。
- 提供CDN加速：可以通过CDN的就近访问原则加快图片的访问速度。
- 第三条也就是最重要的一条，就是方便写文章。

## 3. 搭建图床

> 这里采用gitee作为图片仓库有两点原因，第一点是因为它是免费的，省去了自己维护服务器的费用。第二是因为它是国内的一个网站，所以相比与github来说，访问速度会更快一些。（后来试了一下github感觉也挺快的，差别并不明显，应该是使用了jsdelivr CDN的缘故）

### 码云图床限制如下

> | 功能特性     | 社区版   | 企业版       |        |              |
> | :----------- | :------- | :----------- | :----- | :----------- |
> | 使用场景     | 开源项目 | 个人私有仓库 | 免费版 | 协作开发     |
> | 总计协作人数 | 不限     | ≦ 5 人       | ≦ 5 人 | 20 ~ 100 人+ |
> | 仓库总容量   | 5G       | 5G           | 5G     | 20 ~ 100G    |
> | 单仓库大小   | 1G       | 500M         | 500M   | 1 ~ 3G       |
> | 单文件大小   | 50M      | 50M          | 50M    | 100 ~ 300M   |
> | 附件总容量   | 3G       | 3G           | 3G     | 10 ~ 50G +   |

### 创建图片仓库

![1618304402333](1618304402333.png)

大部分是用  [传送门](https://github.com/Molunerfinn/PicGo/releases)

![1618304505269](1618304505269.png)





我用的是utool的图床

![1618304605851](1618304605851.png)

## 从此你就有了自己的图床了