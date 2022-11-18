---
layout: post
title: 更新PVE系统(未订阅)
date: '2022-11-18 22:48:00'
tags:
- PVE
---

# 更新PVE系统(未订阅)

家里的虚拟化集群中，之前因为esxi那套东西vc占用资源过多，所以切换到pve系列，但是pve在非订阅的情况下无法获得更新。最近需要使用到ubuntu22，但是安装的pve是7.1，不支持ubuntu22的容器，只能运行虚拟机，感觉有点浪费，所以尝试找一个靠谱的repo来升级pve，在此记录过程。

首先，pve的升级依赖apt，而订阅后才能从官方订阅源获取到pve的更新，如果未订阅则只能更新Debian系统内的东西。既然是基于repo，那就应该会有人做repo镜像。经过查找，找到如下的repo可以用来升级。

`注意，下面的操作会直接向文件中写入数据，如有必要记得备份原始文件`

``` bash
# 首先替换debian系统下的repo，默认的会很慢
# 下面4个源四选一即可

# 清华云：
cat > /etc/apt/sources.list <<EOF
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-updates main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-updates main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-backports main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian/ bullseye-backports main contrib non-free
deb https://mirrors.tuna.tsinghua.edu.cn/debian-security bullseye-security main contrib non-free
deb-src https://mirrors.tuna.tsinghua.edu.cn/debian-security bullseye-security main contrib non-free
EOF

# 阿里云：
cat > /etc/apt/sources.list <<EOF
deb http://mirrors.aliyun.com/debian/ bullseye main non-free contrib
deb http://mirrors.aliyun.com/debian/ bullseye-updates main non-free contrib
deb http://mirrors.aliyun.com/debian/ bullseye-backports main non-free contrib
deb-src http://mirrors.aliyun.com/debian/ bullseye main non-free contrib
deb-src http://mirrors.aliyun.com/debian/ bullseye-updates main non-free contrib
deb-src http://mirrors.aliyun.com/debian/ bullseye-backports main non-free contrib
deb http://mirrors.aliyun.com/debian-security/ bullseye-security main non-free contrib
deb-src http://mirrors.aliyun.com/debian-security/ bullseye-security main non-free contrib
EOF

# 华为云：
cat > /etc/apt/sources.list <<EOF
deb https://mirrors.huaweicloud.com/debian/ bullseye main contrib non-free
deb https://mirrors.huaweicloud.com/debian/ bullseye-updates main contrib non-free
deb https://mirrors.huaweicloud.com/debian/ bullseye-backports main contrib non-free
deb https://mirrors.huaweicloud.com/debian-security/ bullseye/updates main contrib non-free
deb-src https://mirrors.huaweicloud.com/debian/ bullseye main contrib non-free
deb-src https://mirrors.huaweicloud.com/debian/ bullseye-updates main contrib non-free
deb-src https://mirrors.huaweicloud.com/debian/ bullseye-backports main contrib non-free
EOF

# 中科大云：
cat > /etc/apt/sources.list <<EOF
deb https://mirrors.ustc.edu.cn/debian/ bullseye main contrib non-free
deb https://mirrors.ustc.edu.cn/debian/ bullseye-updates main contrib non-free
deb https://mirrors.ustc.edu.cn/debian/ bullseye-backports main contrib non-free
deb https://mirrors.ustc.edu.cn/debian-security/ bullseye/updates main contrib non-free
deb-src https://mirrors.ustc.edu.cn/debian/ bullseye main contrib non-free
deb-src https://mirrors.ustc.edu.cn/debian/ bullseye-updates main contrib non-free
deb-src https://mirrors.ustc.edu.cn/debian/ bullseye-backports main contrib non-free
deb-src https://mirrors.ustc.edu.cn/debian-security/ bullseye/updates main contrib non-free
EOF


# 然后，我们添加一下清华下的pve订阅源
# 貌似只有清华有pve的订阅源

# 获取pve源密钥并写入/etc/apt/trusted.gpg.d/proxmox-release-bullseye.gpg
wget http://mirrors.ustc.edu.cn/proxmox/debian/proxmox-release-bullseye.gpg -O /etc/apt/trusted.gpg.d/proxmox-release-bullseye.gpg

# 更改PVE源并写入/etc/apt/sources.list.d/pve-install-repo.list
echo "deb http://mirrors.ustc.edu.cn/proxmox/debian/pve bullseye pve-no-subscription" >/etc/apt/sources.list.d/pve-install-repo.list
```
