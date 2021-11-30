---
title: 静态文件打包exe 全流程(nw) 
date: 2021-11-29 15:39:41
tags: nw
categories: nw
index_img: /img/nw.png
---
nw 他不仅可以操作系统还可以操作node  生成exe 可视化   适合静态文件演示

一下亲测的打包demo 流程

```
https://nwjs.org.cn/download.html
```

下载sdk 版本

![1638164689398](1638164689398.png)

**步骤 1.** 创建 `package.json`:

```json
{
  "name": "helloworld",
  "main": "index.html"
}
```

###  使用 NW.js APIs

NW.js中的APIs都被加载到 `nw`全局对象中 , 并能够在JavaScript代码中直接使用 . 更多细节请参考[API文档](https://nwjs.org.cn/doc/References)

本例子以 `index.html`为例展示了如何创建应用菜单:

```html
<!DOCTYPE html>
<html>
<head>
  <title>上下文菜单</title>
</head>
<body style="width: 100%; height: 100%;">

<p>右击显示上下文菜单</p>

<script>
// 创建一个空菜单
var menu = new nw.Menu();

// 添加菜单项
menu.append(new nw.MenuItem({
  label: '项 A',
  click: function(){
    alert('You have clicked at "项 A"');
  }
}));
menu.append(new nw.MenuItem({ label: '项 B' }));
menu.append(new nw.MenuItem({ type: 'separator' }));
menu.append(new nw.MenuItem({ label: '项 C' }));

// 监听事件
document.body.addEventListener('contextmenu', function(ev) {
  // 阻止显示默认菜单
  ev.preventDefault();
  // 点击处弹出定义的菜单对象
  menu.popup(ev.x, ev.y);

  return false;
}, false);

</script>  
</body>
</html>
```

3 运行测试

![1638164854775](1638164854775.png)

app 是自己新建目录 内容包含package.json   和 index.html

直接拖动到app 文件到nw.exe 可运行 或者 在此目录 运行命令 nw app

### **3打包**

app 打包成zip 注意无嵌套 没有再嵌套app 一层（可以在app 文件夹下全选然后压缩到app ）

![1638165112149](1638165112149.png)

把app.zip 改成app.nw  放在nw.exe 同一层

![1638165216047](1638165216047.png)

 　将app.nw文件移动到和nw.exe同级目录下，然后执行命令**copy /b nw.exe+app.nw app.exe**，这时是可以直接执行app.exe的，但换到其它目录就不可以执行了，因为换到其它目录找不到nwjs包内的依赖文件  生成app.exe

 **【但只能在当前环境执行，在别处使用时需要打包，生成 .exe文件】** 



别人电脑也想使用打包方式

```
https://enigmaprotector.com/en/downloads.html
```

下载

![1638165410299](1638165410299.png)

打包

![1638165443597](1638165443597.png)

输入 是app.exe 

输出自己定

**依赖文件选择**   全选文件 然后排出自己新建的文件 拖放进去 执行打包 

![1638165560689](1638165560689.png)

然后就成功了

![1638165626587](1638165626587.png)