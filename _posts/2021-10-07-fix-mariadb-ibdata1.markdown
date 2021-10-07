---
layout: post
title: 修正mariadb报错：Unable to lock
date: '2021-10-07 10:08:00'
tags:
- Mariadb
---

# 修正Mariadb报错Unable to lock ./ibdata1 error: 11

家里的服务器，应用的Data储存在Nas上，通过nfs挂载到K8S中，然后像mariadb这种对数据及其敏感的应用，在Nas意外宕机的情况下会无法再次启动。
国庆期间，我的Nas因过热宕机过一次，然后mariadb启动就有了如下报错

``` log
[ERROR] InnoDB: Unable to lock ./ibdata1 error: 11
[Note] InnoDB: Check that you do not already have another mysqld process using the same InnoDB data or log files.
```

经过尝试，处理方式如下：

首先，停掉mariadb，然后进入mariadb的储存目录，通过ls检查目录下的文件列表

``` shell
➜  db ls

aria_log.00000001  ib_buffer_pool  ibtmp1             nextcloud
aria_log_control   ibdata1         multi-master.info  performance_schema
core.1             ib_logfile0     mysql
```

我们可以看到那个无法锁定的文件，我们通过改名并复制一份回来的方案，可以解决该文件无法锁定的问题
如果遇到别的文件锁定失败，也执行上面相同的操作即可。

``` shell
mv ibdata1 ibdata1.bak
mv ib_logfile0 ib_logfile0.bak
cp -a ibdata1.bak ibdata1
cp -a ib_logfile0.bak ib_logfile0
```

注意：可能需要root权限才能执行以上操作

然后我们再次启动mariadb即可
