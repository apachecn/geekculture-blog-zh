# 探索 Go sync。作为缓存的池

> 原文：<https://medium.com/geekculture/go-sync-pool-as-cache-in-kubernetes-4e247c52e732?source=collection_archive---------2----------------------->

## 关于同步的详细信息。池和有效使用它的想法

![](img/85d103472ea55fd5df0acd234ab2044e.png)

From unsplash, @[Joshua J. Cotten](https://unsplash.com/photos/aaUMGx6YguU)

我在 JSON 第三方库 orchard 忙工作的时候被一个“苹果”砸到了头。

`sync.Pool`被这么多 libs 用作缓存来提高性能，比如`go-json`通过`sync.Pool`存储一个`sliceHeader` ，以防解码 JSON 时需要重用。为什么我不申请…