# K8s —集群架构

> 原文：<https://medium.com/geekculture/k8s-cluster-architecture-ff6e96f34ca?source=collection_archive---------3----------------------->

## 日常一点 K8s 知识！

![](img/60c279601754d0bff6447fe0ff9c77d9.png)

一个 K8s 集群由一个主节点和一个节点组成，其上运行着几个 K8s 服务。

# 主网点

主服务器是 K8s 集群的大脑，运行以下守护进程服务:`kube-apiserver`、`kube-scheduler`、`kube-controller-manager`、`etcd`和 pod 网络(如 Calico)。我为…画了下图