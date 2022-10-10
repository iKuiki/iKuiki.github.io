---
layout: post
title: 更新容器中的nextcloud
date: '2022-10-10 18:08:00'
tags:
- NEXTCLOUD
- K8S
---

# 更新容器中的nextcloud

我的nextcloud构建于k8s容器中，升级时如果遇到需要执行occ指令，则会因为用户身份原因导致无法执行occ，此时则需要使用sudo切换用户为www-data
但由于使用的是alpine镜像，所以系统里默认无sudo命令，此时则需要通过如下命令安装sudo

``` bash
apk add sudo
```

安装好sudo后即可通过sudo切换为www-data身份执行occ命令
但php执行时需要环境变量PHP_MEMORY_LIMIT，所以sudo命令执行时还需要带上-E把当前环境下的环境变量带上

``` bash
sudo -E -u www-data ./occ xxxx
```

这样就可以顺利执行occ命令了
