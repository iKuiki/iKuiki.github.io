---
layout: post
title: 在win10的ubuntu子系统中自如的使用docker
date: '2016-12-24 12:00:00'
tags:
- ubuntu
- docker
- win10
---

自从win10有了ubuntu子系统，在windows上也可以更方便的使用linux环境了。但是由于win10的linux子系统是被定制的不完全的linux环境，所以无法使用linux内的docker host，只能在windows下使用windows版本的docker，这就给linux环境带来了不便，许多脚本与makefile中的docker命令都无法正常运行。解决这个问题的方案，可以给docker配置默认访问对象为tcp协议的localhost。

经查阅，commandline版本的docker的配置可以通过环境变量来配置。
[https://docs.docker.com/engine/reference/commandline/cli/](https://docs.docker.com/engine/reference/commandline/cli/)

通过在~/.zshrc（或~/.bashrc）中添加以下语句可以解决docker访问的deamon对象：
``` bash
export DOCKER_HOST=127.0.0.1
```
然后在linux中就可以直接使用docker命令访问windows下的docker守护进程了
