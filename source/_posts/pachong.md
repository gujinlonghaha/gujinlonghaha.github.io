---
title: 前端不知道接口就不能爬到数据吗？直接爬html
date: 2021-04-25 14:45:46
index_img: /img/1619768691307.png
tags: cheerio html
categories: cheerio html
---

#### 一般前端都是调用接口，但是有时候碰到服务端页面一展示就是页面没有接口，怎么办呢？直接爬

今天的主角

![1619768989419](1619768989419.png)

比如随便找一个网站测试

![1619769044711](1619769044711.png)

分析html

![1619769095924](1619769095924.png)

编写代码

```js
const axios = require("axios");
const cheerio = require("cheerio");
const fs = require("fs");

axios.get('https://www.taptap.com/top/download',{
}).then((res)=>{
    // console.log(res.data)
  const $ = cheerio.load(res.data);
  let hotList = [];
//   console.log($("#topList"))
  $("#topList .taptap-top-card").each(function (index) {
    if (index !== 0) {

      const $td = $(this).children('.top-card-middle').eq(0);
      console.log($td)
      const link = $td.find("h4").text();
      const text = $td.find(".card-middle-author").find('a').text();
      const hotValue = $td.find(".card-middle-author").find('a').attr('herf');
      hotList.push({
        index,
        link,
        text,
        hotValue,
      });
    }
  });
  fs.writeFileSync(
    `${__dirname}/hotSearch.json`,
    JSON.stringify(hotList),
    "utf-8"
  );


})
```

结果输出

![1619769160692](1619769160692.png)