# 解开 Kubernetes 中持久卷的回收策略

> 原文：<https://medium.com/geekculture/untangling-retain-policies-for-persistent-volume-in-kubernetes-d7c3c07670f4?source=collection_archive---------13----------------------->

## 了解该策略如何影响 Kubernetes 集群内部的数据管理方式

![](img/2427a08822ee4cf2bc102b46d823ddb6.png)

Photo by [benjamin lehman](https://unsplash.com/@benjaminlehman?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如您所知，Kubernetes 上的一切都很好，直到您面临有状态的工作负载并且需要管理其数据。当我们谈到 Kubernetes 为游戏带来的所有强大功能时，会面临许多挑战…