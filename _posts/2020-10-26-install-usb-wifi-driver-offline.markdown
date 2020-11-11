---
layout: post
title: 在离线服务器上安装usb wifi网卡的驱动
date: '2020-10-26 16:34:59'
tags:
- Linux
- Ubuntu
- Network
---

# 在离线服务器上安装usb wifi网卡的驱动

最近在公司遇到一件事情，要给一台Linux的机器配置USB的Wi-Fi网卡，且这台服务器是没有联网的。在此将过程流水账记录下来，方便下次用

因为编译安装是需要先安装make等文件的，所以应当以联网的方式先安装编译的基础依赖，或者找一台别的机器将需要的包下载好后来此机安装（会很麻烦，相当麻烦）

安装依赖

``` sh
# 因为这台机器是想要用来做监控屏，所以先安装界面
sudo apt install xfce4 xinit xdm
# 安装完成后重启即可
reboot
# 为了方便显示中文，安装中文包
sudo apt install language-pack-zh-hans tty-wqy-zenhei

# 安装make相关组建
sudo apt install make gcc git

# 安装udev
sudo apt install udev

# 安装network-manager
sudo apt install network-manager

# 安装wicd
sudo apt install wicd
```
注：如果上面的安装中遇到错误，执行```sudo apt --fix-broken```来修复

将以下git项目的压缩包复制到目标机器并解压
https://github.com/brektrou/rtl8821CU.git

``` sh
# 安装驱动
cd rtl8821CU
make
sudo make install
```

大部分无线网卡，为了方便Windows安装驱动，都会支持2种usb连接方式，默认为光驱模式，里面有windows的驱动文件，然后装好驱动后会自动切到网卡模式，在Linux下我们需要手动将其切换到网卡模式

``` bash
# 列出当前usb设备
lsusb
# 找到其中网卡对应的那个设备（如果无法确定，就先拔掉，然后lsusb后再插上再lsusb一次）
# 然后找到他的id的后面半串，例如我这里为0bda:1a2b，则执行下面的指令
sudo usb_modeswitch -KW -v 0bda -p 1a2b
```
执行完后再次使用lsusb时可以看到该设备的id已经变了，变成网卡了

然后我们就可以使用Wi-Fi manager来进行Wi-Fi的管理了，联网后，即可继续别的操作。

如果在Wi-Fi manager下，找不到你的网卡，则打开Wi-Fi manager的偏好设置，看看是不是wireless的设备是不是为空，如果是，则将Wi-Fi网卡的设备名写入其中即可。
