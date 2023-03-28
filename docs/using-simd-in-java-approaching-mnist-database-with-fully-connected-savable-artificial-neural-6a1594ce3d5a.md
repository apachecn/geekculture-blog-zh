# 在 Java 中使用 SIMD，用完全连接的、可保存的人工神经网络接近 MNIST 数据库

> 原文：<https://medium.com/geekculture/using-simd-in-java-approaching-mnist-database-with-fully-connected-savable-artificial-neural-6a1594ce3d5a?source=collection_archive---------9----------------------->

快速参考:

*   **SIMD** ，单指令多数据的首字母缩写，这里指的是您的 CPU 在单个时钟周期内对几对数字应用相同数学函数的能力。
*   **AVX_512** ，主要被称为**英特尔**的“神奇指令”集(根据莱纳斯·托沃兹提出的半开玩笑的公开过度批评)，将这个神经网络的速度提高了几倍，因为它…