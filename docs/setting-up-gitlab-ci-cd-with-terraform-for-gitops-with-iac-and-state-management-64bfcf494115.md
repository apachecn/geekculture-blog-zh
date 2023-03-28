# 使用 Terraform 设置 GitLab CI/CD，用于具有 IaC 和状态管理的 GitOps

> 原文：<https://medium.com/geekculture/setting-up-gitlab-ci-cd-with-terraform-for-gitops-with-iac-and-state-management-64bfcf494115?source=collection_archive---------2----------------------->

![](img/366ea22c87f72c463c9d28905d9bcb02.png)

Photo by [Artem Sapegin](https://unsplash.com/@sapegin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 基础设施即代码| GitLab | AWS | HashiCorp

我目前使用 GitOps 方法和 ArgoCD 的 Kubernetes 部署，但是为什么不用 IaC 呢？因此，这确实是这篇文章的内容。这篇文章是关于如何设置您当前的或新的 Terraform 项目在…