---
layout: post
title: 在aws的ec2实例上安装nvidia tesla k80驱动
date: '2017-01-09 11:41:00'
tags:
- nvidia
- aws
---

在亚马逊的aws上申请的ec2实例，装好系统时是不带nvidia驱动的，需要我们自己安装驱动
首先从nvidia官网下载tesla系列驱动
``` bash
wget http://cn.download.nvidia.com/XFree86/Linux-x86_64/375.20/NVIDIA-Linux-x86_64-375.20.run
```
然后还需要手动安装Nvidia驱动所需的依赖
``` bash
sudo apt-get update && sudo apt-get install -y gcc make linux-image-extra-virtual
```

然后给予刚下载的NVIDIA-Linux-x86_64-375.20.run运行权限，并运行即可
``` bash
chmod +x NVIDIA-Linux-x86_64-375.20.run
sudo ./NVIDIA-Linux-x86_64-375.20.run
```
