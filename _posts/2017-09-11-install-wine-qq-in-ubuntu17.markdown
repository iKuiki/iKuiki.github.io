---
layout: post
title: Ubuntu17下使用Wine安装QQ
date: '2017-09-11 01:08:17'
tags:
- ubuntu
---

Ubuntu开发虽好用，但是常用软件的匮乏，成为Ubuntu普及的最大软肋
我们国内几乎人人都有的QQ、微信就是重中之重。
微信暂无解决只法，还好QQ可以使用wine QQ获得比较完整的体验

首先，我们使用apt安装wine与winetricks
``` bash
sudo apt install wine winetricks
```

wine是一款可以在ubuntu下通过模拟windows系统api，来达到原生执行exe的软件，但其对于一些大型软件，提供的dll存在版本错误、缺失的问题，常常为了安装一个软件需要折腾很久。而winetricks就是为解决这个问题而诞生的软件，它将大量常用软件的安装脚本集合起来，你需要安装某个软件时只需要用winetricks，它就会帮你配置好你的wine环境。

首先我们先初始化一个wine环境
``` bash
WINEARCH=win32 wine wineboot
```

然后在终端敲入winecfg，将windows版本设置为windows7
![winecfg](http://ow3b21yit.bkt.clouddn.com/wineqq/winecfg.png)

然后我们开始安装qq的依赖包（也可以跳过此步骤直接安装winetricks内置的qq7.8）
``` bash
# Install Core Fonts
winetricks corefonts cjkfonts
# Install Windows Components
winetricks msxml6 riched20 riched30 vcrun6
```

由于winetricks年久失修，在上面步骤中会发生一大堆文件无法下载的情况，请从下面下载后放到*$home/.cache/winetricks*下对应目录中
文件名  |  下载地址
-------|----------
InstMsiW.exe | [Download](http://ow3b21yit.bkt.clouddn.com/winetricks/InstMsiW.exe)
W2KSP4_EN.EXE | [Download](http://ow3b21yit.bkt.clouddn.com/winetricks/W2KSP4_EN.EXE)
arial32.exe | [Download](http://ow3b21yit.bkt.clouddn.com/winetricks/arial32.exe)
arialb32.exe | [Download](http://ow3b21yit.bkt.clouddn.com/winetricks/arialb32.exe)
comic32.exe | [Download](http://ow3b21yit.bkt.clouddn.com/winetricks/comic32.exe)
courie32.exe | [Download](http://ow3b21yit.bkt.clouddn.com/winetricks/courie32.exe)
georgi32.exe | [Download](http://ow3b21yit.bkt.clouddn.com/winetricks/georgi32.exe)
impact32.exe | [Download](http://ow3b21yit.bkt.clouddn.com/winetricks/impact32.exe)
times32.exe | [Download](http://ow3b21yit.bkt.clouddn.com/winetricks/times32.exe)
trebuc32.exe | [Download](http://ow3b21yit.bkt.clouddn.com/winetricks/trebuc32.exe)
verdan32.exe | [Download](http://ow3b21yit.bkt.clouddn.com/winetricks/verdan32.exe)
webdin32.exe | [Download](http://ow3b21yit.bkt.clouddn.com/winetricks/webdin32.exe)


当安装完成已上依赖后，我们就可以直接运行qq的安装程序了
``` bash
LC_ALL=zh_CN.utf8 wine /path/to/installer.exe
```

安装完成后，可以下载下面[QQ-Security-Check-Patch](http://ow3b21yit.bkt.clouddn.com/wineqq/QQ-Security-Check-Patch.zip)
然后将其移进qq安装目录，并执行
``` bash
mv QQ-Security-Check-Patch-10.0.exe ~/.wine/drive_c/Program\ Files/Tencent/QQ/Bin
LC_ALL=zh_CN.utf8 wine ~/.wine/drive_c/Program\ Files/Tencent/QQ/Bin/QQ-Security-Check-Patch-10.0.exe
```
执行后点击应用即可
跳过此步骤可能导致重启后因qq安全检查模块无法启动而导致qq无法使用

现在你已经可以通过以下命令启动QQ：
``` bash
LC_ALL=zh_CN.utf8 wine ~/.wine/drive_c/Program\ Files/Tencent/QQ/Bin/QQ.exe
```

然后你可以为qq创建一个桌面图标：
``` UbuntuDesktop
[Desktop Entry]
Categories=Network;InstantMessaging;
Exec=env LC_ALL=zh_CN.utf8 wine /home/your-username/.wine/drive_c/Program\ Files/Tencent/QQ/Bin/QQ.exe
Icon=/path/to/logo.png
NoDisplay=false
StartupNotify=true
Terminal=false
Type=Application
Name[en_US]=QQ
Name[zh_CN]=QQ
```


**已知问题：无法保存密码**