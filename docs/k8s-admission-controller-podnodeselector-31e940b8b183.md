# K8s 准入控制器—节点选择器

> 原文：<https://medium.com/geekculture/k8s-admission-controller-podnodeselector-31e940b8b183?source=collection_archive---------3----------------------->

## 将命名空间分配给专用工作节点

![](img/cae6efd853f3f2a6151bc094c082a7bc.png)

当您托管多租户 K8s 集群时，您肯定会遇到每个`client/team/customer`一个名称空间的用例。您希望通过一个简单的产品来强制客户端使用特定的工作节点。以便一个客户端的 Pods 在客户端的专用工作节点上运行。