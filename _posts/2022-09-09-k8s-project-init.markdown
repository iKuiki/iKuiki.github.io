---
layout: post
title: K8S初始化命令记录
date: '2022-09-09 19:44:00'
tags:
- K8S
---

# K8S初始化命令记录

昨天K8S集群又一次因为频繁过热轮番宕机遇难了，记录一下灾后重建命令，防止下次遇到

``` bash
# 初始化集群，1.77为集群中负载均衡所在地址，版本1.22.2根据实际情况修改，cidr为cni组件flannel使用
sudo kubeadm init --control-plane-endpoint "192.168.1.77:6443" --upload-certs --kubernetes-version 1.22.2 --pod-network-cidr=10.244.0.0/16

# 部署cni组件
kaf kube-flannel.yml
# 此处根据k8s init后的参数，将其余master、node加入集群

# 进入metallb所在目录，部署metallb
kaf namespace.yaml
kaf config.yaml
sh genSecret.sh
kaf metallb.yaml
```
