# Kubernetes 中的逐层群组

> 原文：<https://medium.com/geekculture/layer-by-layer-cgroup-in-kubernetes-c4e26bda676c?source=collection_archive---------2----------------------->

## 探索关于 Cgroup 的实现细节

![](img/08b99e6e2b3c8b19b7d6951c724188b2.png)

from Unsplash, [@frankiefoto](https://unsplash.com/@frankiefoto)

那些精通云开发的人会很容易认识到 Linux [cgroup](https://man7.org/linux/man-pages/man7/cgroups.7.html) 为 Docker 和 Kubernetes 等容器技术奠定了基础。

Kubernetes 是一个管理和编排大量容器的工具。在配置或部署 pod 时，用户可以…