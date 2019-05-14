---
layout: post
title: 新服务器安装常用环境脚本
date: '2017-11-07 02:49:19'
tags:
- ubuntuserver
---

偶尔要在新服务器安装常用环境，整理以下脚本备用
``` bash
# OS: Ubuntu 16.04 x64 server
apt update
apt install -y zsh git vim curl python3-pip exuberant-ctags cmake gcc g++ \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
# oh-my-zsh
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
# evervim
curl -sLf https://raw.githubusercontent.com/LER0ever/EverVim/master/Boot-EverVim.sh | bash
# docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
apt update
apt install -y docker-ce
# docker-compose
curl -L https://github.com/docker/compose/releases/download/1.17.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
```