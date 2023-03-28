# K8s — Kube-proxy

> 原文：<https://medium.com/geekculture/k8s-kube-proxy-13a2a6e80364?source=collection_archive---------2----------------------->

## 每天一点 K8s 知识！

![](img/a7fd008045a2ea0909b664416426794e.png)

我们知道容器的特点是快速创建和销毁。K8s 豆荚和容器一样，只有一个暂时的生命周期。Pod 可以随时终止或漂移，并随着群集的状态而变化。

一旦 Pod 改变，Pod 提供的服务也不可访问。**如果直接访问 Pod，连续性和** …