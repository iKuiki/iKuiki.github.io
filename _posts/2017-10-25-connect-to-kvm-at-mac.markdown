---
layout: post
title: 在Mac下管理KVM虚拟机
date: '2017-10-25 02:18:57'
tags:
- kvm
- mac
- vnc
- virtual
---

kvm虚拟机的管理客户端virt-manager只提供了linux端，这就给我这个用mac的人带来了困扰。不过既然是linux平台下的软件，那就一定有跨平台解决方案。

一开始我选择的方案是vnc到kvm host，然后在kvm host安装virt-manager，然后来管理。然而管理虽然是可以管理了，但是我发现它却无法远程操作，在远程操作虚拟机时所有键盘操作都无法正确的被转发。
上网爬了下解决方案，意外发现原来kvm的虚拟机是可以直接通过vnc协议连接的，那事情就简单了，我们进入kvm虚拟机的配置，添加一个Graphics设备，Type我们选VNC Server，Address选All Interfaces（其实就是监听0.0.0.0），可以指定一个Ports，最好指定密码（在我这不指定密码无法连接），然后就用VNC Client连接即可。
需要管理虚拟机硬件的时候，就用vnc连接到kvm host然后在用virt-manager就可以，当然你也可以选择在终端使用virsh来管理，显得更有逼格一些，不过我刚接触，还是先用图形化界面熟悉下比较好😄