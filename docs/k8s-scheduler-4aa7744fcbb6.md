# K8s —调度程序

> 原文：<https://medium.com/geekculture/k8s-scheduler-4aa7744fcbb6?source=collection_archive---------5----------------------->

## 每天一点 K8s 知识！

![](img/4c469a5c932a871856ca79fcc2f17ede.png)

K8s `kube-scheduler`负责为集群中的节点分配和调度 pod。它监听`kube-apiserver`，查询尚未被分配节点的 pod，然后根据调度策略将节点分配给这些 pod。

`kube-scheduler`在制定计划时需要充分考虑以下因素…