---
layout: post
title: 如何在使用chrome爬虫时绕过机器人检测
date: '2022-11-21 11:08:00'
tags:
- Golang
- Crawler
- Selenium
- Chromedp
---

# 如何在使用chrome爬虫时绕过机器人检测

最近在使用基于chrome的爬虫时，遇到了疑似浏览器机器人检测拦截的功能，具体就是，通过爬虫控制的浏览器无法进行操作，任何操作都会直接被检测然后503，但是如果把地址复制到正常浏览器中就没有问题。经过在网上的查询，发现网上已经有对应问题的解决方案，不过是python版本，出自此[文章](https://www.fengzifz.com/2021/06/17/hide-python-selenium-spider-feature/)

首先我们使用golang的chromedp包打开chrome并转向上文中提到的[机器人检测页面](https://bot.sannysoft.com/)，我们发现我们通过chrome打开的页面只有WebDriver(New)这一项是红色，所以我们只需要解决不让浏览器上报<当前会话基于自动化>这个状态即可。在Python + selenium中，是在启动时加入如下参数，即可避免浏览器自动上报

``` python
options.add_argument('--disable-blink-features=AutomationControlled')
```

经过测试，在golang下只需要给chromedp传入如下参数即可

``` golang
	// 初始化chromedp
	options := []chromedp.ExecAllocatorOption{
		chromedp.Flag("headless", false),                       // 设置为false时展示窗口
		chromedp.Flag("blink-settings", "imagesEnabled=false"), // 不加载图片以节约资源
		chromedp.Flag("disable-blink-features", "AutomationControlled"),
	}
	options = append(chromedp.DefaultExecAllocatorOptions[:], options...)
	c, _ := chromedp.NewExecAllocator(context.Background(), options...)
```

这样chrome就不会上报错误了。
顺带，在js的puppeteer中使用如下代码来设置

``` js
let browser = await puppeteer.launch({
  //使用无头模式，默认为有头(true为无界面模式)
  headless: false,
  ignoreDefaultArgs: ['--enable-automation'],
  args: ['--disable-blink-features=AutomationControlled'],
  //设置打开页面在浏览器中的宽高
  defaultViewport: {
    width: 1000,
    height: 800,
  },
});
```
