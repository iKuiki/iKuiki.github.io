---
layout: post
title: 在Ubuntu16.04下安装xfce4与vnc
date: '2017-01-17 05:57:00'
tags:
- ubuntu
- vnc
---

本文章适用于Ubuntu16.04，若需要为Ubuntu14.04安装，参考skys的文章：[新服务器之-vncserver搭建](https://blog.skys215.com/index.php/archives/76/)

若服务器在国内，建议切换apt-get的源为国内源：
``` bash
echo 'deb http://cn.archive.ubuntu.com/ubuntu/ xenial main restricted universe multiverse
deb http://cn.archive.ubuntu.com/ubuntu/ xenial-security main restricted universe multiverse
deb http://cn.archive.ubuntu.com/ubuntu/ xenial-updates main restricted universe multiverse
deb http://cn.archive.ubuntu.com/ubuntu/ xenial-backports main restricted universe multiverse' > /etc/apt/sources.list
```

然后安装xfce-4与vncServer:
``` bash
sudo apt-get update && sudo apt-get install xfce4 vnc4server -y
```
启动vncServer：
``` bash
vncserver
```
初次启动需要设定密码
在使用vncserver时会有错误提示，可以通过安装语言包解决：
``` bash
sudo apt-get install language-pack-zh-hans -y
```

启动vnc后，默认端口为5901，连接后发现只有一个terminal，并没有启动桌面环境，可以通过修改配置文件设定启动vnc后需要打开的程序。编辑~/.vnc/xstartup
原文件内容如下：
```
#!/bin/sh

# Uncomment the following two lines for normal desktop:
# unset SESSION_MANAGER
# exec /etc/X11/xinit/xinitrc

[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
xsetroot -solid grey
vncconfig -iconic &
x-terminal-emulator -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &
x-window-manager &
```
通过注释第十、十一行可以避免vnc启动时自动打开一个终端与vncconfig
添加一下代码到末尾可以在启动时自动开启xfce4桌面环境：
```
xfce4-session &
startxfce4 &
xfce4-panel &
```

编辑完成后文件如下：
```
#!/bin/sh

# Uncomment the following two lines for normal desktop:
# unset SESSION_MANAGER
# exec /etc/X11/xinit/xinitrc

[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
xsetroot -solid grey
#vncconfig -iconic &
#x-terminal-emulator -geometry 80x24+10+10 -ls -title "$VNCDESKTOP Desktop" &
x-window-manager &

xfce4-session &
startxfce4 &
xfce4-panel &
```

通过以下命令重启vnc后生效
``` bash
# 结束vnc，数字1为终端号，默认为1
vncserver -kill :1
# 启动vnc
vncserver
```
