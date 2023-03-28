# K8s —如何使用 Secret

> 原文：<https://medium.com/geekculture/k8s-how-to-use-secret-4e8936c50090?source=collection_archive---------4----------------------->

## 每天一点 K8s 知识！

![](img/a869ddb11f689be839a836710f5f3f23.png)

在我的上一篇文章《[K8s——秘密](/geekculture/k8s-secret-3de9bb8e2bf1)》中，我介绍了 K8s `Secret`的概念，我们为什么需要它，`Secrets`的类型以及如何创造。今天，我们来探讨一下如何在 K8s 中使用`Secrets`。

# 将机密装载为卷

Pods 可以通过音量或者环境变量来使用`Secret` 。让我们来看看下面的 YAML…