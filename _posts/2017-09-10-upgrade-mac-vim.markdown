---
layout: post
title: 在Mac下将vim升级到8.0
date: '2017-09-10 16:35:13'
tags:
- mac
- vim
---

Mac下系统自带的vim仅为7.4，如果想用YouCompleteMe插件，就必须要用更新版本的vim，要升级vim到8.0，只需使用homebrew安装即可
``` bash
brew install vim --override-system-vim
```
如果此方法没用（brew vim已安装的情况下）
则让brew升级vim到最新版即可
``` bash
brew upgrade vim
```