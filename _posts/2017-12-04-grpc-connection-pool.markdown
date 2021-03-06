---
layout: post
title: grpc连接池
date: '2017-12-04 09:39:56'
tags:
- grpc
- code
---

从我开始使用grpc到现在，也有不短的一段时间了，然而我还是没有把grpc的基本用法摸透。最近刚好又在看grpc，就来对grpc的连接做个研究

google官方提供的例子中，只有最普通的使用方式：创建连接，访问远程方法，销毁连接
然而在实际项目中，绝不可能如此简单，往往涉及到高并发、高频调用等问题，所以我们就需要考虑到连接性能的问题。
在访问数据库时我们也会碰到类似的瓶颈，在访问量大时，一个连接根本无法满足使用需求，而每次建立新连接的话，很快服务器的可用连接数就会消耗殆尽，所以我们使用连接池来解决这个问题。
这次，我们也要在grpc中尝试使用连接池，来帮助高频客户端提高连接性能。

---

首先，我们来对不同的连接方法能体现出的性能做个测试，测试使用官方的helloworld例子，改成多次调用。
为了测试，我们在以下方面做了改动：
服务端在响应前sleep 1s
客户端通过多种方式重复访问服务端（见表格中描述），来检测性能

|方法|是否并发|服务端sleep时间|访问数|成功数|失败数|耗费时间|
|-|-|-|-|-|-|-|
|使用同一个grpc连接|是|1s|1000|1000|0|10.015325122s|
|使用同一个grpc连接|是|1s|10000|10000|0|100.131993775s|
|使用同一个grpc连接|是|0s|1000|1000|0|24.081929ms|
|使用同一个grpc连接|是|0s|10000|10000|0|279.445219ms|
|每次创建grpc连接|是|0s|10000|3990|6010|1.923255655s|
|每次创建grpc连接|否|0s|10000|5377|4623|2.411625408s|
|使用三方连接池|是|0s|10000|3148|6852|1.820526703s|

从第一条数据上看，我推测，grpc本身应该是实现了连接池的，否则这里的耗费时间应当是1000s以上，而只花费了10s，我推测grpc自带的连接池默认是100并发的
通过监控tcp连接数，还发现grpc自身实现的连接池，只会产生一个tcp连接，也许不应该叫他连接池，而应该称之为允许并发数更好