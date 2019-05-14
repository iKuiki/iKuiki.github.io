---
layout: post
title: Golang使用http包的Client访问https报x509.SystemRootsError错误
date: '2017-03-31 09:54:00'
tags:
- code
- golang
---

前两天在写的一个api在服务器上跑着，使用http包访问https时报x509.SystemRootsError而无法拿到回包，但是同样的代码在本地却运行正常，纠结好久后终于找到错误原因：由于服务器使用瘦环境，所以没有安装https根证书服务，导致golang无法验证https服务器的身份。

解决这个办法的方法有2个，一个是在服务器中安装https根证书服务（推荐），另一个是关闭golang http包的https安全验证
方法一很简单，一般curl包就包含了根证书，所以直接安装curl即可（ubuntu下通过测试，apline可能不行）
方法二则无需改动服务器环境，但可能面临安全问题。做法是通过传config构建一个自定义的http Client，方法如下：
``` go
package main

import (
    "fmt"
    "net/http"
    "crypto/tls"
)

func main() {
    tr := &http.Transport{
        TLSClientConfig: &tls.Config{InsecureSkipVerify: true},
    }
    client := &http.Client{Transport: tr}
    _, err := client.Get("https://golang.org/")
    if err != nil {
        fmt.Println(err)
    }
}
```
