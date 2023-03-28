# AWS EKS 的简单但可扩展的“蓝绿色部署”

> 原文：<https://medium.com/geekculture/simple-yet-scalable-blue-green-deployments-in-aws-eks-87815aa37c03?source=collection_archive---------3----------------------->

## 仅仅通过使用像 CodeBuild 和 CodePipeline 这样的 AWS 服务。

# 介绍

本文解释了一种为部署在 EKS 的应用程序启用蓝绿色部署的方法。我们将使用现有的 AWS 服务，如 CodeBuild 和 CodePipeline，来促进 Kubernetes 应用程序的蓝绿色部署。

# 等等，如果我已经被保险了呢？