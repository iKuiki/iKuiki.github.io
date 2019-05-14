---
layout: post
title: joystick for pmgo
date: '2018-11-15 03:11:37'
tags:
- game
- pokemon
---

宝可梦Lets Go要发售了，发售前不如来玩玩pmgo
不过pmgo毕竟在大陆锁区，那我们如何在大陆玩到pmgo呢？
首先要解决网络问题，pmgo必须使用vpn，并且必须是pmgo发售地区的vpn才可以，所以搭个梯子是必不可少的
怎么搭梯子就不讨论了
搭好梯子后，可以登录pmgo了，却发现登录进来周围一个Pokemon都没有，因为pmgo锁区，大陆是不会刷出Pokemon的
所以我们还需要针对定位进行欺骗
安卓新版本系统中封堵了不少以前能用的虚拟定位的方法，经过网上各种查阅，总结出一个能在2018年11约使用的摇杆虚拟定位方法，搭配Pokemon简直不能更完美了

当前使用设备系统环境如下：
手机：小米4 联通3G版
系统：LineageOS 14.1 （20181107）
安卓版本：7.1.2
安卓安全补丁级别：2018年10月5日

大概思路如下：
使用Magisk获取root权限并且不让pmgo检测到
使用gps Joystick来模拟定位
说起来很简单，做起来还是有点麻烦的

首先安装Magisk，安装步骤自行Google
我安装的版本为17.1（with 6.0.1 manager）
安装好后，在magisk hide中勾选pmgo，并且在magisk设置中隐藏Magisk Manager
安装gps Joystick，并且启用隐私模式来隐藏自己，并卸载原程序
安装一个能将app切换到system分区的app，例如幸运破解器、Link2SD之类的，然后将gps Joystick移动到system分区，以便关闭系统的定位服务
在gps joystick中进入设置，开启「禁用位置服务」选项
重启
然后就可以在gps joystick中操作gps位置了

PS: 安装Magisk是为了给Link2SD授权root权限来将gps joystick移入system分区，如果你自己可以把joystick移入系统分区，可以不安装Magisk、Link2SD（例如通过recovery、adb root之类的方法