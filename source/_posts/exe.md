---
title: 异常环境下的环境访问
date: 2018-05-07 16:05:14
tags: 谷歌浏览器  electron
---

## 当我们碰到客户环境下谷歌浏览器版本导致我们的问题时怎么办呢?



1. 升级用户浏览器 （备份用户数据 书签收藏夹）

2.  安装便捷版谷歌浏览器 （连个谷歌浏览器同时存在用户电脑  数据独立）

3.   使用electron 包装应用 最简单 便捷 

###   前两种就不说了 这里介绍下第三种electron

​    先使用 

```
yarn create electron-app my-app
```

 然后 

```
cd my-app
yarn start
```

接着把这里

![1617783333020](1617783333020.png)

```
  const mainWindow = new BrowserWindow({
    width: 1920,
    height: 1080,
    fullscreen:true,//全屏展示
    center: true, // 窗口居中
    resizable: true, // 窗口大小是否可改变
    maximizable: true, // 窗口是否可以最大化
    autoHideMenuBar:true  //关掉原始操作 重要
  });

  // and load the index.html of the app.
  // mainWindow.loadFile(path.join(__dirname, 'index.html'));
  mainWindow.loadURL('https://share.arena.360.cn/view/5bb1fbd0a438e17f394e284c2aa0bdac')

  // Open the DevTools.  开发者工具
  // mainWindow.webContents.openDevTools();
};
```

最后打包

```
npm run publish
```

在目录下exe 就生成了

![1617783408979](1617783408979.png)

看下效果

![1](1.gif)



好处呢 不言而喻 直接给客户exe  

1. 文件简单直接  
2. 运行默认全屏 干净整洁
3.  同时内核版本自己控制
4. 不会有其他标签页尤其适合大屏演示

