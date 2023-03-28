# Docker —限制容器 CPU 的使用

> 原文：<https://medium.com/geekculture/docker-limit-container-cpu-usage-11eb8ee0de5a?source=collection_archive---------2----------------------->

## 每天一点集装箱知识！

![](img/abfe4ff3a1b50be56ea8b71c971fcd4f.png)

在我的上一篇文章中，我展示了如何通过[限制容器](/geekculture/docker-limit-container-memory-usage-6ef641533cc2)的内存使用，在这篇文章中，让我们来看看如何限制容器 CPU 的使用。

# 容器 CPU 组

让我们快速回顾一下容器`CPU Cgroup`。`CPU Cgroup`是 Cgroups 的 Cgroups 子系统之一…