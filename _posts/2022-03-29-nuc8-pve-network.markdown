---
layout: post
title: nuc8安装pve后网卡间歇性断线
date: '2022-03-29 11:04:00'
tags:
- PVE
---

# nuc8安装pve后网卡间歇性断线

我在nuc8i5ben安装pve7.1后，网卡间歇性断线，表现为机器直接键鼠操作没问题，但是网络用一两天就会断，推测为网络驱动有问题，上网查询后，发现输入如下指令暂时关闭网卡部分功能后，就稳定了，特此记录（注意，每次开机后，都需要再次执行）

``` shell
ethtool -K eno1 gso off gro off tso off tx off rx off rxvlan off txvlan off sg off
```
