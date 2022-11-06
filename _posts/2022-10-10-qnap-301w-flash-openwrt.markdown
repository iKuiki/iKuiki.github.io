---
layout: post
title: 为QNAP-301w刷写openwrt固件
date: '2022-11-05 20:08:00'
tags:
- QNAP-301w
- Openwrt
---

# 为QNAP-301w刷写openwrt固件

家里用的路由器是qnap的301w，最近家里的智能家居偶尔掉线，不知是否和qnap的网络有关，所以我看到有人给qnap写了openwrt的固件包，那就刷入试试。

[固件来源](https://www.youtube.com/watch?v=fsal98GPBNM)

[免拆刷写方案](https://github.com/0x5826/QNAP-QHora-301w-Guide)

## 301w的mmc分区表

|分区|备注|大小|说明|
|---|---|---|---|
|/dev/mmcblk0p1 | "0:HLOS" | 16MB | 系统1 kernel 分区|
|/dev/mmcblk0p2 | "0:HLOS_1" | 16MB | 系统2 kernel 分区|
|/dev/mmcblk0p3 | "0:HLOS_2" | 16MB | 系统3 kernel 分区（空的，官方未刷入固件，也无法引导到该分区，忽视）|
|/dev/mmcblk0p4 | "rootfs" | 512MB | 系统1 rootfs 分区|
|/dev/mmcblk0p5 | "rootfs_1" | 512MB | 系统2 rootfs 分区|
|/dev/mmcblk0p6 | "rootfs_2" | 512MB | 系统3 rootfs 分区（空的，官方未刷入固件，也无法引导到该分区，忽视）|
|/dev/mmcblk0p7 | "0:WIFIFW" | 4MB | 无线的firmware分区|
|/dev/mmcblk0p8 | "reserved" | 16MB | 保留分区（没用的）|
|/dev/mmcblk0p9 | "rootfs_data" | 2.1GB | rootfs的数据分区，多个官方系统共用的数据分区，目前openwrt固件没有使用该分区|

系统1和系统2的分区（kernel+rootfs）是互为备份的，理论上是一毛一样的。

## 开启ssh

原厂系统开启ssh需要按住路由器后面的wps键12秒（响两声），然后就可以用22200端口登陆，用户名admin，密码即网页管理密码。

## 刷机的一些tips

刷机之前，首先要备份一下出厂自带的系统，以防以后需要恢复。可以通过以下命令来备份系统自带固件
刷机过程中，mmc我们需要操作p1、p4这两个分区，另外写入10G网口驱动时我们需要写入mtd10。
另外，虽然301w是双系统，但是好像主要用的都是系统0（即我们要刷入的分区），而系统1是出厂时带的旧系统，所以我们如果想在系统1使用新系统，则也需要把系统0的数据写入系统1，所以我们要备份的分区包括p1、p2、p3、p4以及mtd10。

使用以下命令备份
``` bash
dd if=/dev/mmcblk0p1 of=/tmp/mmcblk0p1.bin
dd if=/dev/mmcblk0p2 of=/tmp/mmcblk0p2.bin
dd if=/dev/mmcblk0p3 of=/tmp/mmcblk0p3.bin
dd if=/dev/mmcblk0p4 of=/tmp/mmcblk0p4.bin
dd if=/dev/mtd10 of=/tmp/mtd10.bin
```

备份完成后将备份文件下载下来

注意，因为tmp内存大小限制，上面的备份文件要分批备份、拷出，主要是p2、p4这两个分区都是512m大小，所以需要分开备份、拷贝，拷贝完成后删除。

## 开始刷机

首先，我们需要进ssh里将系统切为系统1，以刷写系统0

``` bash
sudo fw_setenv current_entry 1
sudo reboot
```

重启后，进入ssh确认是否已经切换到系统1

``` bash
sudo fw_printenv -n current_entry
# 如果上面命令输出1代表已经在系统1，可以继续
```

然后我们就可以将固件刷入系统0
刷机时，将固件（此处必须先用上面QSDK的固件 kernel.bin 和 rootfs.bin）传入/tmp目录下（别的地方也可以）
然后使用dd命令写入分区

``` bash
sudo dd if=/tmp/kernel.bin of=/dev/mmcblk0p1
sudo dd if=/tmp/rootfs.bin of=/dev/mmcblk0p4
sudo fw_setenv current_entry 0 # 设置使用系统0启动
sudo fw_setenv boot_0 good
sudo reboot # 重启
```

刷机完成重启到openwrt后，可以进入openwrt后台，默认密码为`password`，进入后，会发现网口顺序有了变化，从左（10G口）到右分别为eth4（10G）、eth5（10G，默认wan）、eth3、eth2、eth1、eth0，我们为了配合原来的习惯，需要将eth3改为wan，eth5并入lan网桥以恢复使用习惯。

然后会发现10G的口都无法使用，这时我们需要刷入10G口的PHY固件到mtd10。将phy固件上传到路由器，然后执行命令刷入

``` bash
# 抹除原ethfw分区 mtd10的数据：
mtd erase /dev/mtd10
# 刷入fw文件：
mtd -n write /tmp/AQR_ethphyfw_5.6.7.mbn /dev/mtd10
# 修改bootcmd环境变量：
fw_setenv bootcmd "aq_load_fw 0; aq_load_fw 8; bootipq"
# 输入下面命令看下是否有这条记录 bootcmd=aq_load_fw 0; aq_load_fw 8; bootipq
fw_printenv
```

如果有，则重启即可

## 刷入其他openwrt固件

刷入上面固件完成后，即可在openwrt下使用sysupgrade刷入其他openwrt固件了。（如果不用这种方式，直接使用dd刷入会遇到刷入后配置无法保存、wifi无法使用等问题）

由于机型名称异常，在web下无法刷入（无法通过验证）。所以在确定没下错固件的前提下，直接将固件传入机器内，然后登陆ssh后使用以下命令刷写即可。

``` bash
# 刷写固件
# 如果需要，也可以加入 -n 标识在刷写完成后不保留配置
sysupgrade -F /tmp/openwrt.bin
```
