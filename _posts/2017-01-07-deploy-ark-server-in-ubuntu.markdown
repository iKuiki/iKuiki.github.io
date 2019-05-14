---
layout: post
title: 在ubuntu下搭建ARK:Survival Evolved服务器
date: '2017-01-07 11:44:00'
tags:
- ubuntu
- gameserver
---

首先，确保screen已安装，它可以用于让服务器在后台运行
``` bash
sudo apt-get update
sudo apt-get install screen -y
```

然后我们需要下载游戏本体，而下载游戏本体则需要下载steam
为方便在终端下操作，我们下载steamcmd
``` bash
# 安装steamcmd依赖包lib32gcc1
sudo apt-get install lib32gcc1 -y
# 下载steamcmd
cd /path/to/steam/install/dir
wget http://media.steampowered.com/installer/steamcmd_linux.tar.gz
tar zxf steamcmd_linux.tar.gz
```

steam下载好后，则用其下载ark游戏本体
``` bash
cd /path/to/steam/install/dir
./steamcmd.sh

# steam更新完成后，进入steamcmd命令模式
login anonymous
force_install_dir /path/to/ark/dir
app_update 376030 validate
# 若下载出错，则继续使用上条命令，可断点续传
quit
```

# 游戏下载好后，先编写一个启动脚本
``` bash
cd /path/to/ark/dir
echo './ShooterGame/Binaries/Linux/ShootGameServer TheIsland?listen?SessionName=HowToArk?ServerPassword=password?ServerAdminPassword=adminpass -server -log' > startup.sh
chmod +x startup.sh
```

# 启动游戏服务器
``` bash
cd /path/to/ark/dir
# 启动screen会话以保证服务器一直在后台运行
screen
./startup.sh
# 之后可以按ctrl+a,ctrl+d将游戏服务器切换至后台
```

为保证服务器能被连接，需要保证以下端口开放：
udp 27015
udp 7777
tcp 32330
