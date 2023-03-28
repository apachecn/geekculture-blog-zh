# 用动态规划实现矩阵链乘法——举例说明

> 原文：<https://medium.com/geekculture/matrix-chain-multiplication-using-dynamic-programming-a-brief-explanation-with-an-example-c565aa21ebec?source=collection_archive---------7----------------------->

在矩阵链乘法问题中，给我们一些矩阵，并要求我们将它们相乘，使得相乘的总次数尽可能少。由于**矩阵乘法是关联的**，我们有几个选择来乘以矩阵链。换句话说，不管我们如何给产品加括号，结果都是一样的。

也就是说，如果我们取 4 个矩阵 A、B、C 和 D，我们将得到

```
((AB)C)D = (A(BC))D =…
```