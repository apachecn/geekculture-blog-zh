# 使用 Kubernetes 在特定名称空间中使用基于角色的访问控制(RBAC)的最低权限访问

> 原文：<https://medium.com/geekculture/least-privilege-access-with-role-base-access-control-rbac-in-a-particular-namespace-with-e9d1b1ba5e8b?source=collection_archive---------0----------------------->

## RBAC | K8s | AWS | Azure | KubeConfig |资源配额|最低特权可访问性

![](img/ecbb75889deaa11e487ba6d29f793d06.png)

Photo by [Kevin Ku](https://unsplash.com/@ikukevk?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在本文中，我们将使用新的 KubeConfig 设置一个新的名称空间，限制对 K8s 环境中开发的访问。我们会…