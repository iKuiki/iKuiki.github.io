---
layout: post
title: Ubuntu下挂载samba共享
date: '2017-05-19 01:05:00'
tags:
- ubuntu
---

在Ubuntu下如果要访问samba共享，最方便的办法是直接将其挂载到本地文件系统。在Ubuntu下挂载samba，使用cifs文件系统，首先安装cifs格式工具：
``` bash
sudo apt-get install cifs-utils
```
然后就可以直接挂载：
``` bash
mount -t cifs -o username=USERNAME,password=PASSWD //192.168.1.1/shares /mnt/share
```
