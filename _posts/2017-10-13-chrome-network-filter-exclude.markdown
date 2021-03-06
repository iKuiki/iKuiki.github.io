---
layout: post
title: chrome network调试中排除特定项
date: '2017-10-13 08:15:35'
tags:
- chrome
- network-debug
- code
---

在chrome中有个非常好用的开发者调试工具，我们经常用其来调试网页。最近我在分析微信网页版协议时遇到一个问题：在调试工具network中，很多我们不关心的请求也都会显示出来，而官方提供的几个过滤器又不足以满足我们的需求，这时我们就可以使用Regex自定义过滤了。

在调试微信网页版时，我想要把与api无关的请求都过滤掉，为了精确排除特定请求，我使用正则来进行排除。要排除的请求包括图片、js、css请求，剩下的基本就是api了。为了达到这一目的，需要的正则如下：
``` regex
^(?!.*\.js)(?!.*\.jpg)(?!.*\.png)(?!.*\.gif)(?!.*\.css)
```
将这段代码填入Filter中，并勾选Regex后，我们就可以发现含有js、jpg、png、gif、css的请求都被排除了。我们可以根据需要，添加更多的排除项。

**注：在新版chrome中，regex选项被取消，如果要启用regex，只需要将regex放在//之间即可**
上面的regex如果要在新版chrome启用，则写为如下：
``` regex
/^(?!.*\.js)(?!.*\.jpg)(?!.*\.png)(?!.*\.gif)(?!.*\.css)/
```