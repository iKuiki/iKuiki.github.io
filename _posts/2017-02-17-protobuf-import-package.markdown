---
layout: post
title: protobuf跨包import
date: '2017-02-17 04:56:00'
tags:
- code
- protobuf
---

在protobuf中，若要import不同package的proto文件，在不同语言中处理方式是不一样的。

---

### Golang
首先，因为protobuf中的import，在编译到go时也会变成go的import，所以protobuf的编译路径也必须与go的编译路径相同。
以单一GOPATH变量为例，若go/src/a/b/c.proto要引用go/src/d/e/f.proto，则在c.proto中这样写import：
``` protobuf
import "d/e/f.proto"
```
然后要编译c.proto时，则进入go/src/a/b目录下，给protoc命令加上-I参数后编译：
``` bash
protoc -I "./" -I "../../" --go_out=./ f.proto
```
这样得到的pb.go文件的import语句就没问题了

---

### Java
在java中，以maven项目为例，按[grpc官方文档](https://github.com/grpc/grpc-java)给pom.xml加上compile插件后，插件默认以src/main/proto作为protobuf编译的根路径，继续以上文的文件为例，则需要将文件按以下目录摆放即可自动完成编译：
``` dirStruct
src
|_main
   |_proto
       |_a
       | |_b
       |   |_c.proto
       |_d
         |_e
           |_f.proto
```
