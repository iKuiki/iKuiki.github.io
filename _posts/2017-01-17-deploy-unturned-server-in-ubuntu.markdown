---
layout: post
title: 在Ubuntu16.04下搭建Unturned原生服务器
date: '2017-01-17 07:01:00'
tags:
- ubuntu
- gameserver
---

在Ubuntu中，命令行下跑原生Unturned相当复杂，要处理一大堆依赖，所以先上一份通过Steam客户端运行的Unturned Server

为了运行界面版Steam，首先要装XServer环境，并且为了便于远程管理，还要安装vnc server
安装vnc server可以参考：[在Ubuntu16.04下安装xfce4与vnc](http://sz.kuiki.cn:8000/2017/01/%e5%9c%a8ubuntu16-04%e4%b8%8b%e5%ae%89%e8%a3%85xfce4%e4%b8%8evnc/)

安装好vnc后，从[steam官网](http://store.steampowered.com/)下载Linux版的[steam](http://repo.steampowered.com/steam/archive/precise/steam_latest.deb)
在终端安装：
``` bash
# 先安装steam的依赖
sudo apt-get install python python-apt zenity -y
# 然后通过dpkg安装steam
sudo dpkg -i path/to/steam_latest.deb
```

安装完成后运行，让steam更新完成后登陆，下载Unturned
下载完成后，右键Unturned，在属性，设置启动选项里添加以下内容：
```
-nographics -batchmode -silent-crashes +secureserver/Unturned
```
其中的Unturned为服务器名字（并非显示在服务器列表的名字，而仅仅是服务器在本地储存时的名字，可以理解为实例的名字）
关于服务器设定，要修改游戏目录下/Servers/ServerName/Server/Commands.dat，参数容后有空再写😄
