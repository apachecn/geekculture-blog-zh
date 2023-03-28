# K8s —服务帐户

> 原文：<https://medium.com/geekculture/k8s-serviceaccount-689f33de4292?source=collection_archive---------3----------------------->

## 日常一点 K8s 知识！

![](img/5d7ac5b776c803f8386dd84c7ef92207.png)

在 K8s 中，服务帐户为在 Pod 中运行的**进程提供一个身份。当我们访问集群时(例如，使用`kubectl`工具)，你被`apiserver`认证为一个特定的**用户账户**(通常是`admin`)。**

除了用户之外，pod 内容器中的进程也可以联系`apiserver`。当他们这样做时，他们被认证…