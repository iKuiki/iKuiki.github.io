---
layout: post
title: VSCode Golang依赖包
date: '2017-12-30 13:50:31'
tags:
- vscode
- golang
---

在VSCode中安装Golang语言的依赖
因为GFW的原因，在VSCode中无法好好安装Golang的依赖，所以需要手动go get来安装
所需包如下
``` Log
go get -u -v github.com/nsf/gocode
go get -u -v github.com/uudashr/gopkgs/cmd/gopkgs
go get -u -v github.com/ramya-rao-a/go-outline
go get -u -v github.com/acroca/go-symbols
go get -u -v golang.org/x/tools/cmd/guru
go get -u -v golang.org/x/tools/cmd/gorename
go get -u -v github.com/rogpeppe/godef
go get -u -v github.com/golang/lint/golint
go get -u -v github.com/derekparker/delve/cmd/dlv
```