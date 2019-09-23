---
layout: post
title: 初始化openwrt
date: '2019-09-23 15:11:34'
tags:
- openwrt
---

# Openwrt

openwrt是一个开放好用的软路由软件，但是原版刚装好时非常简洁，需要安装、配置各种插件才可堪大用

## 添加ssl支持

在刚装好的原版openwrt系统上，默认是没有ssl支持的，所以使用腾讯提供的镜像源来安装软件都变得不可能（腾讯只提供了https的镜像），所以我们需要为openwrt添加上ssl支持
为了做到支持ssl，只需要安装上ssl相关组件即可

``` bash
opkg update
opkg install libustream-openssl ca-certificates
```

## 替换腾讯源

默认的openwrt源由于国内网络环境的问题访问会极慢，替换为腾讯提供的镜像会好很多

在网页版System/Software>Configuration>Distribution feeds中，将地址中的
```
http://downloads.openwrt.org/releases
```
替换为
```
https://mirrors.cloud.tencent.com/lede/releases
```
即可

## 汉化界面

在网页版System/Software页面搜索luci-i18n-*-zh-cn，在列表中搜索到的汉化组件中安装base、firewall两个组件即可将默认界面汉化完毕
