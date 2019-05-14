---
layout: post
title: 在kvm上安装FreeNas
date: '2017-10-25 02:34:05'
tags:
- freenas
- nas
- kvm
- ubuntuserver
---

KVM是Linux下的非常方便好用、可以使用终端控制的虚拟机平台，在Linux下可以发挥非常不错的效能。我最近又在纠结nas，想要一个方便好用的nas，但是nas系统基本都是以操作系统的方式提供的，而我又没钱买那么多机器，所以虚拟机是一个不错的解决方案。

前置步骤：
- 安装kvm虚拟机平台
- 下载FreeNas镜像

首先进入kvm host的终端，使用以下命令来创建一台kvm虚拟机
``` bash
virt-install --name freenas --memory 4096 --vcpus sockets=1,cores=2,threads=2 --disk device=cdrom,path=/kvm/iso/FreeNAS-11.0-U4.iso --disk path=/kvm/images/freenas.img,size=10,bus=virtio --disk path=/kvm/images/freenas_1.img,size=50,bus=virtio --disk path=/kvm/images/freenas_2.img,size=50,bus=virtio --network bridge=br0,model=virtio --noautoconsole --accelerate --hvm --graphics vnc,listen=0.0.0.0,port=20006 --video vga --input tablet,bus=usb
```
**注，以上参数请更具实际需要修改，以上参数主要设置了以下信息**
- cpu: 1颗双核双线程处理器（共4逻辑处理器）
- cdrom 请改为自己下载的freenas镜像的路径
- disk 本例子中共创建了3块磁盘（一块系统盘10G，2块数据盘各50G，需要注意的是系统盘不能用来存数据）
- 网络 桥接到br0接口（请自己用ifconfig看自己机子里网桥的名字叫什么）
- graphics 一个vnc图形显示器，端口20006
- video 一个vga显示器

运行时记得看返回，如果有报错就针对报错信息来改你的参数
创建好之后，在virt-manager中应当也能看到这台虚拟机了，然后用virt-manager还是vnc来安装就随你了，装完后记得卸载光驱，然后启动即可