# 使用 SparseML 在 CPU 上实现 GPU 级性能

> 原文：<https://medium.com/geekculture/achieve-gpu-grade-performance-on-cpus-with-sparseml-c75879ef0771?source=collection_archive---------5----------------------->

![](img/41a0ac31e0f44e28f68eeee817303d3f.png)

Photo by [Patrik Kernstock](https://unsplash.com/@pkernstock?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

部署是机器学习项目最重要的方面，因为大多数模型最终都被丢弃，要么是因为它们不够快，要么是因为部署在服务器上太昂贵。每个组织都希望构建一个准确且部署成本低廉的模型。他们还希望在合理的时间内满足用户的请求。与他们的 GPU 相比，CPU 服务器很便宜…