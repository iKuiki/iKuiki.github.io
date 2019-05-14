---
layout: post
title: 解决switch无法联机的问题
date: '2017-11-22 08:42:29'
tags:
- nintendoswitch
- vpn
---

最近买了switch，但是switch一直无法联机
经过查询，很多帖子都指向一个问题：**NET Type**
先来科普一下NET Type（我自己理解的也不深，不一定完全准确）
说到NET Type，就要先来说一下NET（网络地址转换）技术
我们都知道，ipv4资源是几十年前就开始使用的东西了，当时也没有考虑到未来会有如此多的网络设备，ipv4资源有限，根本无法满足所有网络设备每台都配备一个网络地址的能力，于是NET技术应运而生。NET技术是让一台设备A介入英特网，然后设备B、C、D通过介入这台设备A来接入因特网，设备B、C、D不直接接入英特网，而靠设备A来转发网络请求，这样就能多台设备共用一个ipv4地址上网了（只有设备A有英特网上的ipv4地址，也叫公网地址）
而国内由于进入互联网时代进入的晚，我国拿到的ipv4资源就更短缺，为了满足十几亿人的上网需求，NET技术在我国是应用最为广泛的。
而在我公司内，更是有这多打3-4层的网络地址转换，在switch的NET Type评级中为C、D的等级，完全无法联机打游戏。

为什么能从eshop下游戏而无法联机打游戏呢？这又要说到switch的联机方式了。为了减轻服务器负担，switch的联机方式不是以任天堂服务器为中心进行游戏数据交换的，而是仅仅以任天堂服务器为中心，组成一个星形网络进行匹配数据交换与心跳检测，然后所有switch两两连接，组成一个环形网络交换游戏数据，每台switch在任天堂服务器的帮助下与其他switch建立p2p连接，这时就需要switch能被别的switch访问到。在别的国家ipv4资源不那么短缺的话，一般NET层数只有一层，switch可以很轻松的通过upnp开启一条与别的switch连接的通道，但国内运营商层层NET，加上网络环境差，就导致了某些地方的网络无法让switch与其他switch建立p2p连接，于是就出现了明明能上网能下游戏，游戏的联网大厅也能进，但是就是无法和别的玩家联机的情况了。

为了解决这个问题，我们就需要提升我们的NET等级，而我所知道的，vpn技术就能实现这一点。（PS：强调一下，请合理运用vpn，不要用来做一些违法的事情）。通过vpn，我们可以将我们的设备连接到一个有公网ipv4的主机下，然后就只有一层网络地址转换（NET Type为B），这时就可以愉快的享受联机游戏了。

---

下面是搭建pptp协议vpn的方法

首先我们需要一台有公网IP的电脑作为服务器，如果你自己家里电脑符合要求，就使用自己家里的电脑，我选择用阿里云的服务器来做这件事。为了降低延迟，我们需要选择与自己地理位置最近的服务器，我选择的是华南1的服务器（请选择国内的服务器，国外的服务器搭的vpn是不能在国内使用的）
出于方便，选用docker技术来搭建pptp服务器
安装docker的教程请自己查

docker镜像选用mobtitude/vpn-pptp镜像
docker-compose.yml文件如下
``` docker-compose
version: '3'

services:
  pptp:
    image: mobtitude/vpn-pptp
    container_name: pptp_server
    privileged: true
    network_mode: host
    volumes:
      - ./secrets:/etc/ppp/chap-secrets
```

然后同目录下还应当有一个secrets文件记录用户信息，内容如下
```
# Secrets for authentication using PAP
# client    server      secret      acceptable local IP addresses
yourName    *           yourPass    *
```

然后运行```docker-compose up -d```即可
但是由于此docker镜像可能过旧？搭好后的pptp服务器处于能连接但是无法上网的状态，通过查看这个项目的issue，发现早已有人给出[解决方案](https://github.com/mobtitude/docker-vpn-pptp/issues/16)
我们只需在这个容器中再运行以下命令即可
``` bash
iptables -A INPUT -i ppp0 -j ACCEPT
iptables -A OUTPUT -o ppp0 -j ACCEPT
iptables -A FORWARD -i ppp0 -j ACCEPT
iptables -A FORWARD -o ppp0 -j ACCEPT
```