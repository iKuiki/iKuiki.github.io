---
layout: post
title: nextcloud经由nginx_proxy时max_size的问题
date: '2018-12-19 07:40:14'
tags:
- docker
- nginxproxy
---

在我自己搭建的nextcloud中，我碰到了过大的文件无法上传的问题，我的nextcloud是搭建在nginx_proxy后方的，因为无法上传的文件大小大约为2m，和nginx的default client_max_body_size很相似，所以我非常有理由怀疑，是因为nginx的这个参数影响了上传
因为我太懒了，一直没去研究，最近在群里一问，张师傅直接给出了解决方案
在nginx_proxy中把vhost引出来，然后在对应域名的vhost中加入这个参数，并加入自己的配置，虽然这样有耦合了，不过至少解决问题了
在nginx_proxy添加对应的env配置项之前，也只能这样了

以下是配置详情

- nginx_proxy docker-compose.yml

``` docker-compose
version: '3'

services:
  nginx-proxy:
    image: jwilder/nginx-proxy
    container_name: nginx-proxy
    restart: always
    volumes:
      - /var/run/docker.sock:/tmp/docker.sock:ro
      - ./certs:/etc/nginx/certs:ro
      - ./nginx/vhost.d:/etc/nginx/vhost.d
      - ./nginx/html:/usr/share/nginx/html
    ports:
      - 80:80
      - 443:443
    network_mode: bridge
    labels:
      - com.github.jrcs.letsencrypt_nginx_proxy_companion.nginx_proxy
  letsencrypt:
    image: jrcs/letsencrypt-nginx-proxy-companion
    container_name: nginx-letsencrypt
    restart: always
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock:ro
      - ./certs:/etc/nginx/certs:rw
      - ./nginx/vhost.d:/etc/nginx/vhost.d
      - ./nginx/html:/usr/share/nginx/html
    network_mode: bridge
```

./nginx/vhost.d/nextcloud.kuiki.cn_location
``` nginx location
client_max_body_size 1024M;
```

如此以来，nextcloud.kuiki.cn的client_max_body_size便被设置到了1G