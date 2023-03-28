# K8s — Pod CPU 资源

> 原文：<https://medium.com/geekculture/k8s-pod-cpu-resource-10653a5a438e?source=collection_archive---------4----------------------->

## 如何控制 Pod CPU 资源的使用

![](img/830a96c449b9f653df9bd4108548cb4b.png)

在 K8s 和容器世界中，有两个基本概念:`Nampespace`和`Cgroups`。我们可以通过`Cgroups`限制资源。`Cgroups`可以分为很多类型，比如 CPU、内存、存储、网络等等。

计算资源是最基本的资源，所有的容器都需要这个资源。让我们深入了解一下容器 CPU…