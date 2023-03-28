# Kubernetes 上的 Trino，Superset 和 Ranger:什么，为什么，如何？

> 原文：<https://medium.com/geekculture/trino-superset-and-ranger-on-kubernetes-what-why-how-c963935fd402?source=collection_archive---------5----------------------->

![](img/2e1b7c8d0fd127e6de35b8e61eaa1acc.png)

Photo by [*Visual Stories || Micheile*](https://unsplash.com/@micheile?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) *on* [*Unsplash*](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

这篇文章是一个自以为是的 SRE 的观点，它是一个开放源码栈，可以轻松地请求、绘制、审计和保护多数据源的任何类型的数据访问。本文是 MLOps 主题系列文章的第一部分。所以，先从理论开始吧！

# Trino 是什么？