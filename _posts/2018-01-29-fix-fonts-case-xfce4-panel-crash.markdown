---
layout: post
title: 解决因字体问题导致xfce4-panel崩溃的问题
date: '2018-01-29 04:24:31'
tags:
- xfce4
- ubuntu
---

最近经常要用vnc来远程我的ubuntu server，但是上面的xfce4桌面环境自从装了chromium之后就没有panel了，看日志，报错如下：

``` log
xfce4-panel: ../../../../src/cairo-scaled-font.c:459: _cairo_scaled_glyph_page_destroy: Assertion `!scaled_font->cache_frozen' failed.
```

可以看出，是字体导致的问题，尝试在xfce4的设置中换了系统的默认设置后，重启vnc，panel终于出来了。
具体设置位置：
右键桌面->Applications->Settings->Appearance->Fonts
我设置为**Monospace**后重启vnc问题解决

另，为了在xfce下支持中文，可以安装一款中文字体，例如：

``` bash
sudo apt install fonts-wqy-zenhei
```

然后将字体设置为**文泉驿等宽正黑**，可以解决panel出现中文崩溃的问题