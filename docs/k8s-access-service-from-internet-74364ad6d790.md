# K8s —从互联网访问服务

> 原文：<https://medium.com/geekculture/k8s-access-service-from-internet-74364ad6d790?source=collection_archive---------3----------------------->

## 日常一点 K8s 知识！

![](img/00d606cfdf290f4f5a2e6607f5341742.png)

除了访问 K8s 集群内部的服务，很多情况下我们还希望应用的服务可以暴露在 K8s 集群外部。在这种情况下，K8s 提供了各种类型的服务，默认的是`ClusterIP`。

# ClusterIP