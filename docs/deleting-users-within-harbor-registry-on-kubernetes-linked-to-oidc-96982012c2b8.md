# 删除链接到 OIDC 的 Kubernetes 上的 Harbor Registry 中的用户

> 原文：<https://medium.com/geekculture/deleting-users-within-harbor-registry-on-kubernetes-linked-to-oidc-96982012c2b8?source=collection_archive---------34----------------------->

![](img/a30adcc5020f1e8aabde942298fc549c.png)

Photo by [Mulyadi](https://unsplash.com/@mullyadii?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 海港| K8s |码头基础

因此当连接到 OIDC 服务时，design Harbor 不允许通过门户删除用户。我目前有一个 OIDC 连接设置，使用 Keycloak 效果很好。但是，您可能会遇到这样的情况，您的用户遇到了某种证书问题，需要重置或…