# K8s 控制器— DaemonSet

> 原文：<https://medium.com/geekculture/k8s-controller-daemonset-226b2ea15088?source=collection_archive---------7----------------------->

## 日常一点 K8s 知识！

![](img/c2a80e6fb34ed7f69e6a2076c547486b.png)

学习完“ [K8s 控制器—部署](https://blog.devgenius.io/k8s-controller-deployment-b1434f17fc9d)”之后，让我们进入下一个概念，“DaemonSet”。

# 达蒙塞特游戏攻略

从控制器的名字就可以看出它的用法:`Daemon`，用来部署守护进程。在 K8s 中，`DaemonSet`用于运行后台进程的副本…