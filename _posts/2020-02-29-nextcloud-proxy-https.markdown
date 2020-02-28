---
layout: post
title: Nextcloud在https反响代理后无法识别scheme
date: '2020-02-29 02:23:59'
tags:
- Nextcloud
- docker
---

# Nextcloud在https反响代理后无法识别scheme

Nextcloud是一款几位好用的webBase网盘，其极佳的客户端可以带来非常友好的体验。

但是今日将其搬到https proxy并且只开https端口后，发现其自动生成的url无法正常访问，总是会生成http协议头的url。经过多番搜索实践，终于发现其需要在```/var/www/html/config/config.php```中做如下配置

``` php
// 生成URL时，Nextcloud会尝试检测服务器是通过https还是http访问的。但是，如果Nextcloud位于代理之后并且代理处理https请求，则Nextcloud无法得知是否正在使用ssl，这会导致生成错误的URL。
// 有效值是http和https。
'overwriteprotocol' => 'https',
```

配置完成后，即可发现Nextcloud会强制生成https形式的url.

更多配置可以查看```/var/www/html/config/config.sample.php```，其中有很多未在config.php中列出的配置。
