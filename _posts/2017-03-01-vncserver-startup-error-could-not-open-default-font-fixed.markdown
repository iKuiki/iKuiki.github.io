---
layout: post
title: vncserver启动出错，提示could not open default font 'fixed'
date: '2017-03-01 01:40:00'
tags:
- ubuntu
- vnc
---

之前在vultr买的ss服务器，装好vncserver与xfce后，启动vncserver失败，查看log文件，关键报错如下：
``` log
Fatal server error:
could not open default font 'fixed'
xsetroot:  unable to open display 'ss.kuiki.cn:1'
/usr/bin/startxfce4: X server already running on display ss.kuiki.cn:1
xrdb: Connection refused
xrdb: Can't open display 'ss.kuiki.cn:1'
```
看得出是因为缺少一个基本字体导致无法启动，Google后得知该字体位于xfonts-base包内，通过apt-get安装该字体包后解决：
``` bash
apt-get install xfonts-base
```
