# 利用 WSL2 和 Linkerd CLI 手动轮换 LinkerD 的证书

> 原文：<https://medium.com/geekculture/rotate-certificates-for-linkerd-manually-utilizing-wsl2-and-the-linkerd-cli-280fe764c24d?source=collection_archive---------12----------------------->

![](img/b8a0e752f0274e1e52de0b98adbc95fa.png)

Photo by [Ben White](https://unsplash.com/@benwhitephotography?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## LinkerD | AWS | Azure | GCP | Kubernetes |网络|原生云

所以我开始在 Kubernetes 集群上看到大量关于 LinkerD 的警告。起初，我无法解释造成这种情况的原因，直到我看到许多 LinkerD pods 的就绪性探测都失败了…