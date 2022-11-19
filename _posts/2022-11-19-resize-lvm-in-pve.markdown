---
layout: post
title: 在pve下给虚拟机的lvm磁盘扩容
date: '2022-11-19 11:04:00'
tags:
- PVE
---

# 在pve下给虚拟机的lvm磁盘扩容

我家虚拟环境中的k8s集群一开始没有分配太多空间，等运行一段时间后发现集群空间并不够用，所以需要尝试扩容。k8s的节点基于lvm安装的系统，一开始只给了一块16G硬盘。虽然可以通过增加硬盘来添加pv以扩容lv，但是有强迫症的我还是想通过扩容原有pv来扩容lv。记录过程如下

## 在pve中扩容原有硬盘

将目标虚拟机关机，在硬件设置中选中原来的磁盘（无需分离，分离了反而无法扩容），选disk action->Resize，然后输入要增加的空间（不是总空间，是要增加的），编辑好后开机

## 编辑分区表扩容原pv所在分区

硬盘增加后，我们还需要编辑分区表以扩容原pv所在分区。输入lsblk，发现原来的硬盘分为2个区，第一个区直接用于/boot挂载点，第二个分区用做pv构成vg，然后划分给lv。

``` bash
# 使用fdisk编辑对应硬盘的分区表
sudo fdisk /dev/sda

# 然后输入p打印当前分区表，记住其中的信息
Command (m for help): p

Disk /dev/sda: 34.4 GB, 34359738368 bytes, 67108864 sectors
Units = sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disk label type: dos
Disk identifier: 0x000e2839

   Device Boot      Start         End      Blocks   Id  System
/dev/sda1   *        2048     2099199     1048576   83  Linux
/dev/sda2         2099200    33554431    15727616   8e  Linux LVM

# 先删除要扩容的分区（只是从分区表中删除，不会影响数据）
Command (m for help): d
Partition number (1,2, default 2): 2
Partition 2 is deleted

# 重新创建一个分区，分区起始位置要与刚刚删掉的一致
Command (m for help): n
Partition type:
   p   primary (1 primary, 0 extended, 3 free)
   e   extended
Select (default p):
Using default response p # 主分区
Partition number (2-4, default 2): # 分区编号，与刚刚删除的一致
First sector (2099200-67108863, default 2099200): # 分区起始位置，必须与刚刚删掉的一致
Using default value 2099200
Last sector, +sectors or +size{K,M,G} (2099200-67108863, default 67108863): # 分区终点，决定了分区大小
Using default value 67108863
Partition 2 of type Linux and of size 31 GiB is set

Command (m for help): w # 把新分区表写入硬盘
The partition table has been altered!

Calling ioctl() to re-read partition table.

WARNING: Re-reading the partition table failed with error 16: Device or resource busy.
The kernel still uses the old table. The new table will be used at
the next reboot or after you run partprobe(8) or kpartx(8)
Syncing disks.

# 到这里，分区表就编辑完成了，输入如下命令使内核重新加载新分区表
sudo partprobe
# 如果上面命令下系统无法重新加载分区表(可通过lsblk检查)，则重启系统以使内核加载新的分区表
sudo reboot
```

## 扩容pv

系统重启后，我们需要先扩容pv

``` bash
# 确认扩容结果
sudo lsblk
# 应该可以看到，sda2的容量已经变成了32G

# 扩容pv
sudo pvresize /dev/sda2

# 确认pv扩容成功
sudo pvdisplay
```

## 扩容lv

完成pv的扩容后，我们vg的池子就已经变大，这时候我们就可以直接扩容lv了。
使用如下命令直接将lv扩容至指定大小

``` bash
# 扩容指定lv
# lv路径可以通过df -h命令查看
# -r代表顺便扩容fs，就不需要再单独运行扩容fs的命令了
# -L代表扩容到指定容量，虽然我们磁盘扩容到32G，但是因为计算方式、内部损耗等原因，实际无法使用到32G，我们可以依次尝试32G、31G、30G，直到找到可以扩容的大小
# 或者如果指定的扩容大小过大，系统会提示有多少extents可用，扩容到指定大小需要多少extents，计算一下就知道可以到多少了。懒的话就逐个往下试就好。
sudo lvextend -rL 29G /dev/mapper/centos-root

# 检查扩容结果
df -h
```

至此扩容就成功完成了。
