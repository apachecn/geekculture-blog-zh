# K8s —入口、入口等级和控制器

> 原文：<https://medium.com/geekculture/k8s-ingress-ingress-class-and-controller-b278396e94d9?source=collection_archive---------1----------------------->

## 日常一点 K8s 知识！

![](img/fa10e1d5ef7cb13a4f4be24acf4f0576.png)

从我之前的文章“T2 从互联网访问 K8s 服务”中，我们知道向外界公开 K8s 服务最简单的方法是使用`NodePort`。例如，您将容器的服务端口绑定到主机的端口，然后使用节点的 IP 加上由`NodePort`指定的端口，您可以访问服务。