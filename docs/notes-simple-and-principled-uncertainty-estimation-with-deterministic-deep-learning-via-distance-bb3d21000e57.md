# [注释]通过远程感知利用确定性深度学习进行简单且有原则的不确定性估计

> 原文：<https://medium.com/geekculture/notes-simple-and-principled-uncertainty-estimation-with-deterministic-deep-learning-via-distance-bb3d21000e57?source=collection_archive---------31----------------------->

原文：<https://arxiv.org/pdf/2006.10108.pdf>

## 1.介绍

以前的不确定性估计方法(深度集成、MC 丢失)有两个缺陷:

*   它们需要多次运行神经网络。
*   决策边界是线性的，即不确定性主要位于决策边界周围。