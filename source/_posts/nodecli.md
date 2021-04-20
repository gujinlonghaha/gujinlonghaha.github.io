---
title: nodecli demo测试
date: 2019-03-20 16:58:53
tags: npm cli node
index_img: /img/1618904854392.png
categories: npm cli node
---

整体流程很简单 但是有几个注意事项：

```
npm init -y
```

```
{
  "name": "gjltestcli",
  "version": "1.0.2",
  "description": "",
  "main": "index.js",
  "bin": "index.js",
  "scripts": {
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "dependencies": {
    "ejs": "^3.1.6",
    "inquirer": "^8.0.0"
  }
}

```

1. **bin 一定要写因为这里写的是命令行程序**

2. **dependencies 安装依赖 特别是打包后的依赖需要 npm install  包名  -S  （要不然别人安完短依赖）**

3. **脚本命令前加 相当于跨平台兼容 让机器识别你的代码**

   ```javascript
   #!/usr/bin/env node
   ```

4. **当你频繁测试的时候不需要频繁发布包  先进入你的包 npm link (相当于连接到全局)  直接可以执行测试**

5. **发布可能重名 注意版本号每次更新 和名字不要重复 （可以搜索查询）**

6. **注册好后登录npm账号。**

   - 

   ```
   npm login
   ```

   **依次输入第二步中第一种方法注册的用户名、密码和邮箱。**

   **登录成功后执行npm发布命令。**

   - 

   ```
   npm publish
   ```

![1618905966155](1618905966155.png)

**demo 就成功了**

补充：

npm link用来在本地项目和本地npm模块之间建立连接，可以在本地进行模块测试

具体用法：

**1. 项目和模块在同一个目录下，可以使用相对路径**

npm link ../module

**2. 项目和模块不在同一个目录下**

cd到模块目录，npm link，进行全局link

cd到项目目录，npm link 模块名(package.json中的name)

**3. 解除link**

解除项目和模块link，项目目录下，npm unlink 模块名

解除模块全局link，模块目录下，npm unlink 模块名