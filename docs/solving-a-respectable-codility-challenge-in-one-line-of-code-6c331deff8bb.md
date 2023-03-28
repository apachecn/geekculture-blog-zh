# 在一行代码中解决一个“值得尊敬的”Codility 挑战

> 原文：<https://medium.com/geekculture/solving-a-respectable-codility-challenge-in-one-line-of-code-6c331deff8bb?source=collection_archive---------22----------------------->

保持事情简单高效。

![](img/edd3e3d7a9955e6728bef9656617e509.png)

Photo by [Joshua Aragon](https://unsplash.com/@goshua13?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 内容

∘ [任务](#ca1e)t5】∘[理解挑战](#a423)
∘ [分步迭代](#7118)
∘ [一套基本的数学运算](#4853)
∘ [移植到 Python](#f528)
∘ [引用](#c27a)

我真的很喜欢挑战。它们是提高你解决问题的技能和编程语言知识的一种极好的(而且完全免费的)方式。它们也是为技术面试“热身”大脑的好方法，尤其是因为你的大多数解决方案都是根据其处理边缘情况的能力进行评估的，同时保持时间效率。

许多现代专业开发都是由配置和链接库组成的，所以深入到一个纯粹基于逻辑和性能的任务中会让人耳目一新。

在过去的几年里，我在周末和假期慢慢地研究他们的课程和挑战，这帮助我保持了寻找“贪婪算法”解决方案并考虑性能和可读性的心态。

一个特别的教训让我大吃一惊。现实任务按照难度的顺序被分为“不痛不痒”、“值得尊敬”或“雄心勃勃”这项任务被评为“值得尊敬”，但它可以通过一行代码有效地解决。

## 任务

编码课程由 PDF 格式的阅读材料和一系列“任务”组成“前缀求和”课程中的第一个任务称为“计算除法”这些说明是:

> 写一个函数…给定三个整数 A，B 和 K，返回范围[A]内的整数个数..B]能被 K 整除

我们保证 A ≤ B。

## 理解挑战

为了更全面地了解一个问题，我有时发现把它勾画出来是很有用的，完全不用担心效率。

一个显而易见的强力解决方案是遍历 A 和 B 之间的所有整数，使用模(%)检查它们是否能被 K 整除。

为了安全起见，我们还需要检查 B！= 0.在 ruby 中:

```
def solution(a, b, k)
  return 0 if b == 0 count = 0
  (a..b).each do |ii| 
    count += 1 if ii % k == 0
  end count
end
```

或者，更简洁地说，使用三元运算符和`filter`给出符合我们标准的所有数字的计数(大小):

```
def solution(a, b, k)
  b == 0 ? 0 : (a..b).filter{ |ii| ii % k == 0 }.size
end
```

这将以线性时间运行— `O(n)` —如果 A 是 0，B 是测试可能的最大值 20 亿，这就不太理想了。

## 分步迭代

类似地，如下例所示，在 A 到 B 的范围内寻找 K 的最小和最大倍数，并在等于 K 的步骤中迭代，仍然会给我们留下`O(n)`——如果 K 为 1，效率特别低。

```
def solution(a, b, k)
  return 0 if b == 0 || k > b first_divisible = a % k == 0 ? a : a + (k - a % k)
  last_divisible = b - b % k
  (first_divisible..last_divisible).step(k).size
end
```

但是我们接近一个有效的解决方案。我们现在可以看到，我们需要看到的是，K 的倍数在 A 和 b 之间出现了多少次。

## 一组基本的数学运算

让我们使用 Codility 中的原始示例，a = 6，b = 11，k = 2

*   我们可以将其向下舍入到 5，以给出 2 均匀地变成 11 的总方式数
*   `(6 - 1) / 2 = 2.5`——对于小于 6 的整数**能被 2 整除的路数，我们可以将其下舍入到 2。我们可以从上面的总数中减去这些。**
*   `5 - 2 = 3` —从总数中减去排除的计数，得到我们的结果。

在 ruby 中，将一个整数除以另一个整数会自动向下舍入商，返回一个整数结果。这使得我们的解决方案既整洁又高效，在`O(1)`恒定时间内运行。

```
def solution(a, b, k)
  b / k - (a - 1) / k
end
```

A 和/或 B 为零的边缘情况也可以通过下舍入除法巧妙处理。

## 移植到 Python

Python 的 floor division(“//”运算符)也可以为我们处理这个操作，尽管我们仍然需要检查 B 是否为 0。

```
def solution(a, b, k):
    return 0 if b == 0 else int(b // k - (a - 1) // k)
```

最后两个例子中的任何一个都将得到 100%的结果。

如果这里有什么不合理的地方，或者你能想到一个更简洁的方法来达到这个效果，请告诉我。

## 参考

 [## 条件前缀求和一课

### 为技术面试做准备，并通过我们的实践编程课程发展您的编程技能。成为一个强大的技术…

app.codility.com](https://app.codility.com/programmers/lessons/5-prefix_sums/)  [## //楼层划分- Python 参考(正确的方法)0.1 文档

### 返回商的整数部分。A // B 任何计算结果为数值类型的表达式。任何表情…

python-reference . readthedocs . io](https://python-reference.readthedocs.io/en/latest/docs/operators/floor_division.html)