---
layout: post
title: 如何获取ESXi的UUID
date: '2021-04-29 20:00:00'
tags:
- ESXi
- VMware
---

# 获取ESXi的UUID

今天配置zabbix中对ESXi的访问时，发现配置zabbix的vmware监控需要获取ESXi服务器的uuid，经过很多查阅，最后发现获取此uuid，最方便的方法应该是打开ESXi控制台后，进入ssh控制台获取信息。
ESXi默认没有开启ssh控制台，在ESXi的首页点【操作】->【服务】->【启用安全Shell】，就可以开启ssh，然后再选择【操作】->【SSH控制台】就会在系统的终端中连接ESXi服务器
键入以下命令，即可获取ESXi服务器的uuid

``` shell
esxcfg-info -u |awk '{print tolower($0)}'
```

