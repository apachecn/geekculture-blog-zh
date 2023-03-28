# K8s —入口介绍第一部分

> 原文：<https://medium.com/geekculture/k8s-ingress-introduction-part-one-696deeff6908?source=collection_archive---------0----------------------->

## 侵入概念和最小侵入示例

在我的[上一篇文章](https://blog.devgenius.io/k8s-service-introduction-b1197f5d0ab4)中，我介绍了 K8s 的“服务”对象，这是 K8s 内置的负载均衡机制。它使用静态 IP 地址代理动态变化的 pod，支持域名访问和服务发现，是微服务架构的必要基础设施。

K8s `Service`很有用，但只能说是“基础设施”。因为它对网络流量的管理方案仍然过于简单，还有很大的不足之处