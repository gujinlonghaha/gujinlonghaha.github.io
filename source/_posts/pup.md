---
title: pupeteer自动化加版本测试
date: 2020-04-08 10:57:47
index_img: /img/1617850474932.png
tags: pupeteer 
categories: pupeteer
---

# 多版本谷歌测试如何进行

没必要不更换谷歌浏览器吧？ 对答案是肯定的！今天主角帮你解决

## puppeteerjs

你可以在浏览器中手动执行的绝大多数操作都可以使用 Puppeteer 来完成！ 下面是一些示例：

- 生成页面 PDF。
- 抓取 SPA（单页应用）并生成预渲染内容（即“SSR”（服务器端渲染））。
- 自动提交表单，进行 UI 测试，键盘输入等。
- 创建一个时时更新的自动化测试环境。 使用最新的 JavaScript 和浏览器功能直接在最新版本的Chrome中执行测试。
- 捕获网站的 [timeline trace](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/reference)，用来帮助分析性能问题。
- 测试浏览器扩展。

### 他其中有个模块

![1617851125113](1617851125113.png)

那个就是版本号 其实你发现和咱们版本号不一样 后来我研究发现是这样的   [地址](https://omahaproxy.appspot.com/)   需要fq

![1617851262264](1617851262264.png)

是第二个箭头也就是分支切换出来的版本号码

那他支持哪些版本呢  [地址](https://npm.taobao.org/mirrors/chromium-browser-snapshots/Win_x64/)   淘宝给出了镜像地址   下面号码拿来直接用

![1617851479500](1617851479500.png)

现在谷歌浏览器都是下载安装器然后在线安装有没离线包 

[福利](https://www.chromedownloads.net/chrome64win-stable/)

![1617851464105](1617851464105.png)



自己写的部分常用脚本 以防忘记

```
const puppeteer = require('puppeteer');
let operte=async(t) => {

    // 开启 browser
   const browserFetcher = puppeteer.createBrowserFetcher();
   const revisionInfo = await browserFetcher.download('533271');  //指定版本
    const vesion= await browserFetcher.localRevisions()
    console.log(vesion)
    let browser = await puppeteer.launch({
        executablePath: revisionInfo.executablePath,
        headless: false,
        defaultViewport: {
            height: 1080,
            width: 1920
        }
    });
    // 新增分页
    console.log(browser)
    let page = await browser.newPage();
    // 到自己的网站
    await page.goto(`${t}`);

     // 等待订阅按钮出现
    //  await page.waitForSelector("button[class='subscribe-button pill-button']");
    //  page.type('#mytextarea', 'Hello', { delay: 100 }); // 立即输入
    //  page.type('#mytextarea', 'World', { delay: 100 }); // 输入变慢，像一个用户
    //  // 点击订阅按钮
    //  await page.click("button[class='subscribe-button pill-button']");
    //   关闭标签页
    // page.close()
    // 关闭浏览器
    // await browser.close();
}

let arr=[
    'https://www.baidu.com/',
    // 'https://cn.bing.com/search?q=b%E6%A1%88&PC=U316&FORM=CHROMN',
]
arr.forEach(t => {
    operte(t)
});
```

