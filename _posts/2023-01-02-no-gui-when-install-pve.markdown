---
layout: post
title: 安装PVE时，无法启动GUI
date: '2023-01-02 13:08:00'
tags:
- PVE
---

# 安装PVE时，无法启动GUI

我给家里的新服务器安装PVE时，发现PVE安装会卡在安装程序启动，并提示no screen found，猜测是因为主板是用的板载2D显卡的缘故，所以需要禁用一些高级显卡功能来兼容。操作如下

启动PVE安装时选择Debug模式安装，然后PVE的安装程序便会在关键节点暂停等你操作后再继续。

第一次暂停时，是安装用的Linux刚启动，还未加在光驱内容，我们不需要在此操作，输入exit或者直接按ctrl+D跳过
第二次暂停时，是已经加载光驱内容，加载GUI之前，我们需要在这里修改x server的配置。

在终端输入命令Xorg -configure，此命令会在系统根目录生成xorg默认配置文件，将其移动到对应目录后编辑
``` bash
mv xorg.conf.new /etc/X11/xorg.conf
vi /etc/X11/xorg.conf
```

需要修改的地方主要有2处，首先搜索glx，找到一行Load "glx"，修改为：
``` conf
Disable "glx"
Disable "glamoregl"
```
然后再搜索modesetting，找到Driver "modesetting"，改为Driver "fbdev"

保存后，同样输入exit或者ctrl+D继续即可，后面按照正常流程安装即可完成
