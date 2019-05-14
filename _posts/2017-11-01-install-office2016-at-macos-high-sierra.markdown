---
layout: post
title: 在macOS high Sierra上安装Office2016的注意事项
date: '2017-11-01 01:49:08'
tags:
- office
- mac
---

新装了系统之后，用之前skys给我分享的Office2016安装包(md5:da19a8d901b369b3dd9fa01bf901f02a)装好后发现无法自动更新，Outlook提示无法在high Sierra上运行

此镜像是一个较旧版本的Office2016批量授权免激活镜像，内含的Office2016为15.13.3版#
已知无法正常运行的功能有：
- Microsoft AutoUpdate(MAU);影响：Office无法升级
- Outlook因版本过旧无法启动

---

## 解决办法：
手动下载新版MAU安装后即可自动升级，升级到15.35之后功能就正常了
地址：[Microsoft AutoUpdate](https://support.office.com/en-us/article/Update-history-for-Office-2016-for-Mac-700cab62-0d67-4f23-947b-3686cb1a8eb7#bkmk_mau)
**ISSUE: 因MAU只检测启动过的Office程序的更新，所以Outlook无法通过MAU升级**

解决Outlook无法使用的方法：
手动下载新版Outlook单独安装后即可
地址：[Office2016 Update](https://support.office.com/zh-cn/article/Office-2016-for-Mac-%E7%9A%84%E5%8F%91%E8%A1%8C%E8%AF%B4%E6%98%8E-ed2da564-6d53-4542-9954-7e3209681a41?ui=zh-CN&rs=zh-CN&ad=CN)

---