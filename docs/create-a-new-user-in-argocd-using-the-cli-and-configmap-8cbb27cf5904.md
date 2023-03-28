# 使用 CLI 和配置图在 ArgoCD 中创建新用户

> 原文：<https://medium.com/geekculture/create-a-new-user-in-argocd-using-the-cli-and-configmap-8cbb27cf5904?source=collection_archive---------0----------------------->

![](img/7314e72dcd0e236f7d4dbc7985523b15.png)

Photo by [Ferenc Almasi](https://unsplash.com/@flowforfrank?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## Kubernetes | ArgoCD CLI

这篇文章将会谈到添加一个用户到 ArgoCD 是多么的简单。但是，过程有点不一样。

## 假设:

*   ArgoCD 安装在 Kubernetes 集群上
*   您可以通过 ArgoCD CLI 访问 ArgoCD