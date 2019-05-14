---
layout: post
title: 创建Macintosh的安装u盘
date: '2018-01-22 09:01:47'
tags:
- mac
---

偶尔要创建mac的安装u盘，命令记录一下免得到处找

``` bash
sudo /Applications/Install\ macOS\ High\ Sierra.app/Contents/Resources/createinstallmedia --volume /Volumes/InstallMacintosh --applicationpath /Applications/Install\ macOS\ High\ Sierra.app --nointeraction
```

其中volume参数以及applicationpath参数需要按照实际情况填写