---
title: 记一次实现帮朋友ik签到
date: 2021-11-26 11:15:10
index_img: /img/iks.png
tags:  
categories: 
---

**朋友需要每天登陆  签到  获取免费流量 我用自己界面打开如下**

![1637908245040](1637908245040.png)

很简单

1 打开浏览器 进入网站

2 输入账号  密码

3 登陆

4 签到

5 关闭浏览器


直接用puppeteer 就可以实现

![1637908391124](1637908391124.png)

核心代码如下

```
const puppeteer = require('puppeteer');
let { config } = require('./config.js');

 var init=async () => {
    const browser = await puppeteer.launch(
        {
            headless: false,
            args: ['--start-maximized'],
            defaultViewport: {
                width: 1920,
                height: 1080
            }
        }
    );
    const page = await browser.newPage();
    await page.goto('https://ik.co/user#', {
        waitUntil: 'domcontentloaded'
    });
    await page.waitForSelector('input[name="email"]');
    await page.type('input[name="email"]', config.email);
    await page.type('input[name="password"]', config.password);
    await page.click('button[type = "submit"]');
    await page.waitForSelector('#checkin-div');
    let content = await page.$eval('#checkin-div a', el => el.textContent);
    console.log(content);
    if (content.includes("明日再来")) {
        page.on('dialog', async dialog => {
            console.log(dialog.message());
            await page.waitFor(3000);
            await dialog.dismiss();
        })
        await page.evaluate(() => alert('已经签到请明日再来,我将自动关闭'));


    } else {
        await page.click('#checkin-div');
        page.on('dialog', async dialog => {
            await page.waitFor(3000);
            await dialog.dismiss();
        })
        await page.evaluate(() => alert('签到成功,我将自动关闭'));
    }
    await page.waitFor(3000);
    await browser.close();

}
exports.init=init;
```

```
let config={
    email: "你的邮箱",
    password: "你的密码",
}
exports.config=config;
```

```
const schedule = require('node-schedule');
let { init } = require('./index.js');


/* // 每分钟第30秒执行
 schedule.scheduleJob('30 * * * * *',()=>{
    init()
}); */
console.log(process.argv.slice(-1)[0])
try {
    if (process.argv.slice(-1)[0] == 'task') {
        // 每周-到周五 9:30执行
        var rule = new schedule.RecurrenceRule();
        rule.dayOfWeek = [0, new schedule.Range(1, 6)];
        rule.hour = 9;
        rule.minute = 30;
        schedule.scheduleJob(rule, function () {
            init()
        });
    }
    if (process.argv.slice(-1)[0] == 'init') {
        init()
    }
} catch (error) {
    
    console.error(error)

}

```

https://github.com/gujinlonghaha/pupdaka

