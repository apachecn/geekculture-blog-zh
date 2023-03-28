# 使用 Finalizer、Informer 和 Webhook 开发高级 Kubernetes 操作符

> 原文：<https://medium.com/geekculture/advanced-kubernetes-operator-development-with-finalizer-informer-and-webhook-2339e9fd8eba?source=collection_archive---------7----------------------->

在以前的文章中，我介绍了 Kubernetes 操作符，使用 Kubebuilder 编写了一个基本操作符，并在后来对它进行了改进。

*   [T3【Kubernetes】初学操作员——什么、为什么、如何 ](/swlh/kubernetes-operator-for-beginners-what-why-how-21b23f0cb9b1)
*   [*高级 Kubernetes 操作员开发*](/swlh/advanced-kubernetes-operators-development-988edad5f58a)

`Operator`本身还有更多值得探索的地方。在本文中，我将重点介绍`finalizer`、informer 和 webhook，以及将它们添加到前面的操作符的主要方法。