# K8s —管理有状态应用程序，第二部分

> 原文：<https://medium.com/geekculture/k8s-manage-stateful-applications-part-two-d4c4e6cd1982?source=collection_archive---------9----------------------->

## 在 K8s 中管理有状态的应用程序

![](img/7b4686c9e7d0369edef677c93574400f.png)

在“ [K8s —管理有状态应用，第一部分](/dev-genius/k8s-manage-stateful-applications-part-one-c9b4f7e74488)”中，我解释了什么是`StatefulSet`，我们为什么需要它，如何部署它以及如何为它创建 K8s `Service`。让我们继续讨论`StatefulSet`的数据持久性。

为了帮助回顾，下图显示了`Service`和……之间的关系