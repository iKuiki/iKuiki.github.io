---
layout: post
title: 在Dockerfile中使用apt-get update后清理产生的垃圾文件
date: '2017-01-02 03:43:00'
tags:
- docker
- dockerfile
---

在Dockerfile中，由于Docker的文件系统是层文件系统，Dockerfile中的每一条命令都将产生一个层，这样每次使用apt-get update && apt-get install后都会产生一些缓存文件。这些缓存文件对于Docker来说属于垃圾文件，假设你上一个层中缓存有apt-get update的结果，那么下次你的Dockerfile运行时就会直接使用之前的缓存，然后继续构建之后的命令，这样你的Docker中的apt软件源就不是最新的软件列表了，将会带来缓存过期的问题。并且这些缓存将占用不少空间，导致最终生成的image非常庞大，而这些垃圾文件是我们最终的image中无需使用到的东西，我们应当在Docker构建过程中予以删除。

基于Dockerfile的层镜像规则，我们的命令必须在同一条Docker指令中完成，也就是使用分行命令的方式完成，所以需要使用apt安装软件的命令应当写为如下形式：（以安装vim为例）
``` bash
apt-get update && apt-get install vim; apt-get autoclean; rm -rf /var/lib/apt/lists/*
```
