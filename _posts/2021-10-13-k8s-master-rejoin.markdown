---
layout: post
title: 移除K8S的master节点并重新加入
date: '2021-10-07 18:04:00'
tags:
- K8S
---

# 移除高可用K8S集群下的master节点并重新加入

在我的K8S集群上，因为之前断电导致的系统故障，被迫回滚了系统，然后K8S节点下的master节点无法同步（master2、master3都挂了，只有master1可用），所以我打算把master2、master3移出集群后重新加入来修复
从K8S将master移除后，并不能简单的通过reset master，然后从新join就解决，在etcd下还有残留的节点信息，必须清除后才能以原来的IP正常的join回来。步骤如下(步骤中，以master3为例)

``` shell
# 首先，在可以管理K8S集群的终端，通过kubectl移除节点
# 腾空该节点
kubectl drain k8s-master3 --ignore-daemonsets
# 移除该node
kubectl delete node k8s-master3

# 登陆到master1下，然后进入etcd的容器，移除节点
# 进入etcd容器
sudo docker exec -it $(sudo docker ps -f name=etcd_etcd -q) /bin/sh
# 在etcd终端下查看etcd成员列表
etcdctl  --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/peer.crt --key=/etc/kubernetes/pki/etcd/peer.key member list
# 移除master3对应的member
etcdctl  --cacert=/etc/kubernetes/pki/etcd/ca.crt --cert=/etc/kubernetes/pki/etcd/peer.crt --key=/etc/kubernetes/pki/etcd/peer.key member remove xxxxxx

# 然后，登陆到master1上，重新生成证书、token
# 生成证书(注意，这句命令执行完会返回证书的certificate-key，需要记下来之后会用到)
sudo kubeadm init phase upload-certs --upload-certs
# 生成join的token(注意，这句命令执行完后会返回之后join时的token与token-ca-cert，需要记录下来之后会用)
sudo kubeadm token create --print-join-command

# 然后登陆到master3上，执行join操作
# join之前首先需要reset
sudo kubeadm reset
# 然后用刚刚上上步返回的命令来join，并加上--control-plane与上面的证书Key
sudo kubeadm join 192.168.1.xx:6443 --token xxxx.xxxxxxxxxx --discovery-token-ca-cert-hash sha256:xxxxxxxxxx --control-plane --certificate-key xxxxxx
```
