---
layout: post
title: use vscode via vnc
date: '2017-10-14 16:09:29'
tags:
- ubuntu
- vnc
- vscode
---

在vnc server中打开vscode时(ubuntu17.04 + xfce4 + vnc4server + vscode deb version)，会发现点了图标无效果，查看vnc日志，得到以下错误：
``` log
Xlib:  extension "XInputExtension" missing on display ":1.0".
```

经过查询，获得以下解决方案：
``` bash
sudo cp /usr/lib/x86_64-linux-gnu/libxcb.so.1.1.0 /usr/share/code/libxcb.so.1
sudo sed -i 's/BIG-REQUESTS/_IG-REQUESTS/' /usr/share/code/libxcb.so.1
```