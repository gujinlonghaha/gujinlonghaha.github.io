---
title: git优化工作流之别名
date: 2019-08-01 11:15:10
index_img: /img/git.png
tags: git  
categories: git  
---



![1637807790323](git.gif)

 **上面骚操作是不是很给力 一个小技巧可以使你的 Git 体验更简单、容易、熟悉：别名** 

 **同事看到你的 Git 工作流时，总是充满了惊讶与好奇：** 

设置命令

```
git config --global alias.co checkout
```

 这意味着，当要输入 `git commit` 时，只需要输入 `git ci`。 随着你继续不断地使用 Git，可能也会经常使用其他命令，所以创建别名时不要犹豫。

当取消别名时可以  全局配置文件的目录 全局叫gitconfig

![1637808629314](1637808629314.png)

![1637808684380](1637808684380.png)

单个项目位置在这叫config  里面有一些核心信息

![1637809849741](1637809849741.png)

找到命令删掉即可



个人常用别名分享

![1637809434803](1637809434803.png)

```
    i = init
	cl = clone
	a = add .
	ca = commit -a
	br = branch
	co = checkout
	cm = checkout master
	st = status
	ca = commit -a
```

这样就可以愉快玩耍了