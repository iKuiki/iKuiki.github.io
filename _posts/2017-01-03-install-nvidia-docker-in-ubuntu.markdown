---
layout: post
title: 在ubuntu16.04下安装Nvidia-docker
date: '2017-01-03 02:16:00'
tags:
- ubuntu
- nvidia
- docker
---

为了在docker中使用Nvidia显卡，我们需要使用Nvidia出的Nvidia-docker。在ubuntu 16.04 x64中安装Nvidia-docker的步骤归纳如下：
首先，必须启用Nvidia的专有显卡驱动，在ubuntu的**设置-》软件与更新-》专有驱动**下启用Nvidia的专有驱动即可。

然后我们需要安装[docker官方维护的apt源下的docker-engine](https://docs.docker.com/engine/installation/linux/ubuntulinux/)

``` bash
# 导入docker的apt源的gpg key
sudo apt-key adv \
               --keyserver hkp://ha.pool.sks-keyservers.net:80 \
               --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
# 添加apt源
echo "deb https://apt.dockerproject.org/repo ubuntu-xenial main" | sudo tee /etc/apt/sources.list.d/docker.list
# 更新apt源
sudo apt-get update
# 安装docker-engine
sudo apt-get install docker-engine
```

安装好docker-engine后，就可以按照[Nvidia-docker项目的说明](https://github.com/NVIDIA/nvidia-docker)安装Nvidia-docker了
``` bash
# Install nvidia-docker and nvidia-docker-plugin
wget -P /tmp https://github.com/NVIDIA/nvidia-docker/releases/download/v1.0.0-rc.3/nvidia-docker_1.0.0.rc.3-1_amd64.deb
sudo dpkg -i /tmp/nvidia-docker*.deb && rm /tmp/nvidia-docker*.deb

# Test nvidia-smi
nvidia-docker run --rm nvidia/cuda nvidia-smi
```
