---
title: 公司文档生成利器
date: 2021-04-04 16:05:14
index_img: /img/1618049229806.png
tags: docsify  文档
categories: docsify  文档
---

个人使用过hexo 等生成器但是这个是真厉害 因为他**不需要**把makdown 转化html 的编译过程**启动极快**

还有几个独特的优势



1. 离线缓存
2. 不需要编译直接部署 可以使用gitee 搭配使用 不需要CL/CD 
3. 支持vue

花了半小时测测试效果如下  真好看  即使是公司的人不会代码也是轻而易举搞定

![1618050136294](1618050136294.png)

![1618050169962](1618050169962.png)

# [开始](https://jingping-ye.github.io/docsify-docs-zh/#/快速上手/开始?id=开始)

首选全局安装`docsify-cli`

```bash
 npm i docsify-cli -g
```

## [初始化](https://jingping-ye.github.io/docsify-docs-zh/#/快速上手/开始?id=初始化)

初始化会自动生成`./docs`文件夹

```bash
docsify init ./docs

// 如果不想新建一个文件夹，可以直接进行初始化操作

docsify init
```

## [建立第一个文档](https://jingping-ye.github.io/docsify-docs-zh/#/快速上手/开始?id=建立第一个文档)

初始化之后目录结构如下

```text
|—— docs
    |—— index.html 入口
    |—— README.md 主页
    |—— .nojekyll 防止Github忽视下划线开头文件
```

## [预览网站](https://jingping-ye.github.io/docsify-docs-zh/#/快速上手/开始?id=预览网站)

执行以下命令，预览网站就在`http://localhost:3000`网址打开

```bash
cd docs
docsify serve
```

# [离线缓存（PWA）](https://jingping-ye.github.io/docsify-docs-zh/#/指南/离线缓存?id=离线缓存（pwa）)

## [1. 创建一个 servieWorker](https://jingping-ye.github.io/docsify-docs-zh/#/指南/离线缓存?id=_1-创建一个-servieworker)

> 根目录下创建`sw.js`文件

```js
/* ===========================================================
 * docsify sw.js
 * ===========================================================
 * Copyright 2016 @huxpro
 * Licensed under Apache 2.0
 * Register service worker.
 * ========================================================== */

const RUNTIME = "docsify";
const HOSTNAME_WHITELIST = [self.location.hostname, "fonts.gstatic.com", "fonts.googleapis.com", "cdn.jsdelivr.net"];

// The Util Function to hack URLs of intercepted requests
const getFixedUrl = (req) => {
  var now = Date.now();
  var url = new URL(req.url);

  // 1. fixed http URL
  // Just keep syncing with location.protocol
  // fetch(httpURL) belongs to active mixed content.
  // And fetch(httpRequest) is not supported yet.
  url.protocol = self.location.protocol;

  // 2. add query for caching-busting.
  // Github Pages served with Cache-Control: max-age=600
  // max-age on mutable content is error-prone, with SW life of bugs can even extend.
  // Until cache mode of Fetch API landed, we have to workaround cache-busting with query string.
  // Cache-Control-Bug: https://bugs.chromium.org/p/chromium/issues/detail?id=453190
  if (url.hostname === self.location.hostname) {
    url.search += (url.search ? "&" : "?") + "cache-bust=" + now;
  }
  return url.href;
};

/**
 *  @Lifecycle Activate
 *  New one activated when old isnt being used.
 *
 *  waitUntil(): activating ====> activated
 */
self.addEventListener("activate", (event) => {
  event.waitUntil(self.clients.claim());
});

/**
 *  @Functional Fetch
 *  All network requests are being intercepted here.
 *
 *  void respondWith(Promise<Response> r)
 */
self.addEventListener("fetch", (event) => {
  // Skip some of cross-origin requests, like those for Google Analytics.
  if (HOSTNAME_WHITELIST.indexOf(new URL(event.request.url).hostname) > -1) {
    // Stale-while-revalidate
    // similar to HTTP's stale-while-revalidate: https://www.mnot.net/blog/2007/12/12/stale
    // Upgrade from Jake's to Surma's: https://gist.github.com/surma/eb441223daaedf880801ad80006389f1
    const cached = caches.match(event.request);
    const fixedUrl = getFixedUrl(event.request);
    const fetched = fetch(fixedUrl, { cache: "no-store" });
    const fetchedCopy = fetched.then((resp) => resp.clone());

    // Call respondWith() with whatever we get first.
    // If the fetch fails (e.g disconnected), wait for the cache.
    // If there’s nothing in cache, wait for the fetch.
    // If neither yields a response, return offline pages.
    event.respondWith(
      Promise.race([fetched.catch((_) => cached), cached])
        .then((resp) => resp || fetched)
        .catch((_) => {
          /* eat any errors */
        })
    );

    // Update the cache with the version we fetched (only for ok status)
    event.waitUntil(
      Promise.all([fetchedCopy, caches.open(RUNTIME)])
        .then(([response, cache]) => response.ok && cache.put(event.request, response))
        .catch((_) => {
          /* eat any errors */
        })
    );
  }
});
```

## [2. 在`index.html`文件中注册](https://jingping-ye.github.io/docsify-docs-zh/#/指南/离线缓存?id=_2-在indexhtml文件中注册)

```html
<script>
  if (typeof navigator.serviceWorker !== "undefined") {
    navigator.serviceWorker.register("sw.js");
  }
</script>
```

