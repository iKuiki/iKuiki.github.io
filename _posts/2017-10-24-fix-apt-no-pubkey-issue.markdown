---
layout: post
title: 修复apt NO_PUBPEY错误
date: '2017-10-24 13:05:42'
tags:
- ubuntu
---

今天在安装open media vault时，选择在debian基础上安装的方式，在添加OMV的apt source之后碰到如下错误
``` error
W: GPG error: http://packages.openmediavault.org/public kralizec InRelease: The following signatures couldn't be verified because the public key is not available: NO_PUBKEY 7E7A6C592EF35D13
W: The repository 'http://packages.openmediavault.org/public kralizec InRelease' is not signed.
N: Data from such a repository can't be authenticated and is therefore potentially dangerous to use.
N: See apt-secure(8) manpage for repository creation and user configuration details.
```

意思是这个安装源的公钥不存在，无法验证该安装源。然而OMV官方并未提供该安装源的公钥。但是在提示中我们知道该公钥指纹为**7E7A6C592EF35D13**，所以我们可以通过以下命令从ubuntu的公钥服务器中获取该公钥：
``` bash
apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 7E7A6C592EF35D13
```
之后再运行apt-get update不会再报NO_PUBKEY的错误了