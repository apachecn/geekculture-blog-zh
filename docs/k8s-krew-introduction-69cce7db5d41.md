# K8s — Krew 简介

> 原文：<https://medium.com/geekculture/k8s-krew-introduction-69cce7db5d41?source=collection_archive---------1----------------------->

## kubectl 的插件管理工具

![](img/1f82d8035aa735a0b9313843d019fb96.png)

Pic from [krew](https://krew.sigs.k8s.io/)

# 什么是克鲁

管理过 K8s 集群的人一定非常熟悉`kubectl`，这是 K8s 的命令行工具。我今天要介绍的是`krew`，`kubectl`的插件管理工具。

`krew`为`kubectl`提供了一个简单的下载、检索和管理其他插件的方法，类似于…