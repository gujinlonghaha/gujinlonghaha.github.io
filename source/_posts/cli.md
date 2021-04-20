---
title: npx和cli的那些事 
date: 2019-04-20 14:58:53
tags: npm cli npx
index_img: /img/1618902265022.png
categories: npm cli npx
---

### **纸上得来终觉浅 绝知此事要躬行**

![1618902764339](1618902764339.png)

备注：gjltestcli 是自己写的命令行工具demo

1. **因为我没有全局安装gjltestcli 所以调用不了命令行**
2. **我使用npx gjltestcli 能成功是因为npx 临时安装gjltestcli （不会出现在当前目录nodemoudle）**
3. **再次使用gjltestcli 可以执行说明npx 是临时安装 没有全局安装所以不能执行**

![1618903114752](1618903114752.png)

1. **本地安装gjltestcli** 
2. **因为是本地安装所以调用失效 （可以使用全局安装 -g）**不是环境变量 也可以加入环境变量 也可以使用npm link
3. **没有全局安装的可以通过 .\node_modules\.bin\gjltestcli  执行**  因为安装后在这个目录
4. ![1618903384161](1618903384161.png)
5. **npx gjltestcli 执行成功是因为 npx 会从nodemoudle 或者本地调用 调用不到就会自己临时安装 不会在你的依赖中安装**

上面说明了什么npx的优点：( 当被下载完，则下载的代码会被擦除。 )

- [轻松地运行本地命令](http://nodejs.cn/learn/the-npx-nodejs-package-runner#轻松地运行本地命令)
- [无需安装的命令执行](http://nodejs.cn/learn/the-npx-nodejs-package-runner#无需安装的命令执行)
- [使用不同的 Node.js 版本运行代码](http://nodejs.cn/learn/the-npx-nodejs-package-runner#使用不同的-nodejs-版本运行代码)
- [直接从 URL 运行任意代码片段](http://nodejs.cn/learn/the-npx-nodejs-package-runner#直接从-url-运行任意代码片段)

后面两点可以自行简单测试 最后一点代表不发包（npm publish ）也可以使用

## 直接从 URL 运行任意代码片段

`npx` 并不限制使用 npm 仓库上发布的软件包。

可以运行位于 GitHub gist 中的代码，例如：

```bash
npx https://gist.github.com/zkat/4bc19503fe9e9309e2bfaa2c58074d32
```

当然，当运行不受控制的代码时，需要格外小心，因为强大的功能带来了巨大的责任。

[友情链接](https://www.ruanyifeng.com/blog/2019/02/npx.html) 

[npx](http://nodejs.cn/learn/the-npx-nodejs-package-runner) 

