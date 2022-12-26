---
layout: post
title: 从PVE集群中移除节点
date: '2022-12-25 23:08:00'
tags:
- PVE
---

# 从PVE集群中移除节点

如果要将一个节点从pve集群中移除，因为pve没有提供GUI，所以需要手动执行。过程记录如下

``` bash
# 先将节点驱逐
pvecm delnode nodename

# 移除剩余的文件
rm -rf /etc/pve/nodes/nodename

# 从以下两个文件中移除节点信息
vi /etc/pve/priv/authorized_keys
vi /etc/pve/priv/known_hosts
```

至此，节点移除就完成了
