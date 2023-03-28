# K8s —控制箱位置

> 原文：<https://medium.com/geekculture/k8s-control-pod-location-90a37a3724e2?source=collection_archive---------7----------------------->

## 日常一点 K8s 知识！

![](img/e611f8c6d5ef36f0d44fa79eb33cbd46.png)

在 K8s 中，默认情况下,`Scheduler`会将 pod 调度到所有可用的节点。但是，在某些情况下，您希望将 pod 部署到指定的节点，例如将具有大量磁盘 I/O 的 pod 部署到配置了 SSD 的节点；或者 Pods 需要 GPU，需要运行在配置了 GPU 的节点上；或属于特定项目的节点。