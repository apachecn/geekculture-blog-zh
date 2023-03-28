# K8s 网络— CNI 简介

> 原文：<https://medium.com/geekculture/k8s-network-cni-introduction-b035d42ad68f?source=collection_archive---------3----------------------->

## K8s 容器网络接口介绍

![](img/74b9040dee9bf20481bdd319e91c25a1.png)

# 什么是 CNI？

CNI 代表“容器网络接口”，它是一个标准的、通用的网络接口。 [CNI](https://github.com/containernetworking/cni) 是 CNCF(Cloud native computing foundation)项目之一，它由一个规范和用于编写插件来配置容器网络接口的库组成。