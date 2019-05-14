---
layout: post
title: 在装有gtx系列显卡的主机安装ubuntuServer版后无法启动的解决方法
date: '2017-05-24 05:58:00'
tags:
- ubuntu
- ubuntuserver
- nvidia
---

经过对开机日志的研究，可以断定是nouveau驱动导致开机失败，但是在recovery模式下能正常启动，那么我们只需要在recovery模式下禁用nouveau驱动，并安装nvidia的闭源驱动即可。
首先新建/etc/modprobe.d/blacklist-nouveau.conf文件并填入以下内容：
```
blacklist nouveau
blacklist lbm-nouveau
options nouveau modeset=0
alias nouveau off
alias lbm-nouveau off
```
然后运行命令update-initramfs -u来刷新启动脚本
接下来，在nvidia下载好官方驱动后安装，装好后重启即可解决
