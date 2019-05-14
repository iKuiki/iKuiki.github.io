---
layout: post
title: 让docker中的时区与主机保持一致
date: '2017-09-26 05:56:22'
tags:
- docker
---

我们每次默认创建的docker，时区都会与主机不同，因为linux使用/etc/timezone与／etc/localtime来记录时区，而docker拥有单独的一整套系统套件，所以时区也是单独的。为了让docker与host的时区同步，我们只需将这两个文件映射进docker即可。
docker-compose写法：
``` docker-compose
    volumes:
      - /etc/timezone:/etc/timezone:ro
      - /etc/localtime:/etc/localtime:ro
```

---

经过使用，我发现上门的方法还有很大局限性：
- 要求docker host必须为linux系统，并且正确的设置了时区
- 如果docker镜像为像alpine这类轻量镜像，没有预装tzdata包，则无效

所以可以使用以下方法来解决：
Dockerfile:
``` Dockerfile
# install tzdata
RUN apk add --no-cache tzdata

# set timezone
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
```