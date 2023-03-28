# K8s —控制器管理器

> 原文：<https://medium.com/geekculture/k8s-controller-manager-9177b383593b?source=collection_archive---------1----------------------->

## 每天一点 K8s 知识！

![](img/a28cbc4a3556275f3a9dbd85827848f2.png)

K8s `Controller Manager`由`kube-controller-manager`和`cloud-controller-manager`组成，是 K8s 的大脑。它通过`apiserver`监控整个集群的状态，确保集群处于预期的工作状态。

`Controller Manager`架构图如下所示: