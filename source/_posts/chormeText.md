---
title: 多端同步验证方案汇总
date: 2021-04-6 15:13:31
index_img: /img/1618298516454.png
tags: 展示  预览  浏览器
categories: 展示  预览  浏览器 
---

BrowserShots

地址：

http://browsershots.org

作为首批跨浏览器测试网站之一，支持多种浏览器，包括一些旧浏览器，如 Lynx、Konqueror 和 Seamonkey。

它会生成屏幕截图，显示你的网站在不同浏览器中的渲染表现，唯一的缺点是需要在线使用该工具。

![图片](640.png)

Browser Sandbox

地址：

https://turbo.net/browsers

它是一款可运行在桌面和平板上的应用程序，可以像运行原生浏览器那样运行多种浏览器。

它支持的浏览器种类很多，包括旧版本的 IE、Canary 及开发版的 IE。

![1618298424509](1618298424509.png)

MultiBrowser

地址：

https://www.multibrowser.com

一款桌面应用程序，支持 IE7 到 IE11、Edge、Firefox 和 Chrome。你可以用它来测试网站的桌面版本和移动版本，可以进行手动测试或自动化测试。

![图片](640.webp)

LambdaTest

地址：

https://www.lambdatest.com

一个在线服务，可用来进行不同平台的跨浏览器测试。例如，你可以测试网站在 Windows、Linux、macOS 上的不同浏览器（Firefox 或 Chrome）中的表现。

它还提供了一个集成调试工具、地理位置工具，可以用来测试本地站点。

![图片](640.png)

Experitest Cross Browser Testing

地址：

https://experitest.com/cross-browser-testing

这个工具可以用来测试网站在不同环境下的兼容性和性能。它还可以与其他服务集成起来，比如 Github、Gitlab、Jenkins、TravisCI 和 CircleCI 等，把网站的部署流程流水线化。

![图片](640-1618298248548.png)

BrowserStack

地址：

https://www.browserstack.com

跨浏览器测试领域响当当的一款工具，被一些大型开源项目采用，比如 jQuery 和 React.js。BrowserStack 列出了数百种浏览器、设备和测试策略，确保你的网站可以在尽可能多的环境中正常运行。

![图片](data:image/gif;base64,iVBORw0KGgoAAAANSUhEUgAAAAEAAAABCAYAAAAfFcSJAAAADUlEQVQImWNgYGBgAAAABQABh6FO1AAAAABJRU5ErkJggg==)

SauceLabs

地址：

https://saucelabs.com

SauceLabs 为各种规模的网站提供了完整的浏览器兼容性测试工具，不管是企业级的、中小型公司还是开源项目。

![图片](640.webp)

CrossBrowserTesting

地址：

https://crossbrowsertesting.com

使用这个工具，可以不写代码进行自动化浏览器兼容性测试，可以测试线上或本地站点，还可以截取屏幕快照和视频。

![图片](640-1618298248490.png)

TestingBot

地址：

https://testingbot.com

TestingBot 为网站和原生移动 App 提供了完整的测试策略，可以在真实的 iOS 或 Android 设备上运行测试。

![图片](640-1618298248633.png)

Browserling

地址：

https://www.browserling.com

如果你想要在 IE 上进行快速测试，BrowserLing 或许是个不错的选择。

你可以用它进行简单的交互式测试，支持一些旧浏览器，比如 IE 10、IE 11 和 Safari 4、Safari 5。

![图片](640.webp)

Comparium

地址：

https://comparium.app

Comparium 提供了一个免费的工具，可以截取不同环境下的屏幕快照，并进行比对。

![图片](640.webp)

Puppeteer

地址：

https://github.com/puppeteer/puppeteer

Puppeteer 是一个 Node.js 模块，提供了与 Chrome 和 Firefox 交互的 API。

你可以用它提供的 API 来截取屏幕快照、生成 PDF、进行自动化交互式测试（比如自动填写表单、键盘输入），整体上可以进行自动化网站测试。

![图片](640-1618298248541.png)

Playwright

地址：

https://github.com/microsoft/playwright

Playwright 是微软设计的一个项目，用于执行自动化浏览器测试。它提供了一个简单的 API。除了可以模拟用户交互，还可以拦截网络请求、模拟移动设备、支持地理位置数据和权限控制。

Playwright 支持基于 Chromium 的浏览器、Firefox 和 Webkit（比如 Safari）。

![图片](640.webp)

Nightwatch.js

地址：

https://nightwatchjs.org

NightWatch.js 是一个用于进行端到端侧二十的 Node.js 模块。它提供了简单易用的 API，可用它检查某个元素是否包含了特定的文本或是否可见，甚至是可以用来测试 CSS 类、CSS ID 和属性。

![图片](640-1618298248649.png)

Cypress

地址：

https://www.cypress.io

Cypress 是一个端到端测试套件，可用来测试和调试现代 Web 应用程序。

它在执行测试的同时还能记录下每一个测试的状态。你可以回溯每一个状态，并比较状态之间都发生了什么变化，这让 Web 应用程序的调试变得很直观。

![图片](640-1618298248526.png)

WebDriverIO

地址：

https://webdriver.io

这是一款 Node.js 自动化测试框架，支持很多 JavaScript 库，比如 React.js、Vue 和 Angular。

因为它是基于 W3C WebDriver 和 Chrome DevTools 的，所以可以在本地运行，也可以在云端运行，就像 SauceLab、BrowserStack 和 TestingBot 那样。

![图片](640-1618298248370.png)

Selenium

地址：

https://www.selenium.dev

Selenium 是一款浏览器自动化测试工具。实际上，它并没有提供现成的测试框架，但可以通过扩展来实现。

很多测试框架、App 或服务，包括上述的一些工具都是基于 Selenium 的。

![图片](640.webp)