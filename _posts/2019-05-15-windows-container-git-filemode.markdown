---
layout: post
title: Windows中使用vscode remote container时遇到的filemode的坑
date: '2019-05-15 15:35:14'
tags:
- windows
- vscode
- docker
---

最具vscode出了remote development套件预览版，可以说彻底打破了我必须用Mac写代码的需求（写代码2大核心需求：类Unix系统，有微信）
它的remote development有3大模式：
- 基于ssh远程开发
- 基于容器本地伪远程开发
- 基于WSL（Windows Subsystem for Linux）的本地伪远程开发

后两者为什么说是伪远程呢，因为他们实际用的还是你本地的主机，只是在本地主机上创建一个虚拟环境，然后再远程连接到这个虚拟环境上开发，然后再把开发目录挂载上来，实际还是跑在本地的。然后，因为这个挂载，在Windows上就带来一个问题，Windows上是没有filemode的概念的，但是git有啊，所有在Windows上clone了linux、MacOS下创建的项目，会经常提示文件有修改，git diff一看，全是filemode由644变为755，这种无意义的更改应当避免，也应当避免在Windows上提交具有错误filemode的文件

解决方法如下：
首先将git设置为fileMode不敏感：```git config core.fileMode false```（注意不可全局设置，因为vscode挂载了全局的gitconfig文件，全局设置会无法写入

在add文件时也要注意，如果add的文件是755权限，但是并没有x权限需求，就应该手动执行```git update-index --chmod=-x xxx.xx```来去除x权限，反之，因为设置了fileMode false，添加文件时默认权限其实是644，遇到需要执行权限的文件则应该手动执行```git update-index --chmod=+x xxx.xx```来赋予x权限
