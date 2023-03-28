# 算法简介:双指针技术

> 原文：<https://medium.com/geekculture/intro-to-algorithms-two-pointers-technique-b37f962eab5?source=collection_archive---------32----------------------->

## 一项基本技术的详细介绍

![](img/d8e1f66799565805285bb6b6ade0f194.png)

Photo by [Etienne Girardet](https://unsplash.com/@etiennegirardet?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/2?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在我的上一篇文章中，我介绍了二分搜索法 T4 算法的来龙去脉。二分搜索法方法假设您正在浏览的集合已经被排序，两个指针技术也不例外。然而，二分搜索法在集合中定位单个元素，而双指针技术用于在数组中定位两个同时满足条件的元素。让我们来看看它是如何工作的。

# 提前收费:一个天真的解决方案

让我们转向一个强力的、天真的解决方案，在这个方案中，两个指针技术可能更适合。

> 给定一个整数数组(已经按升序排序)，查找并返回两个元素的索引，这两个元素相加后等于所提供的目标值。

或者给定一个`array`和一个`target`，写一个算法求解`array[i] + array[j] === target`。

我们如何设计一个[强力](https://en.wikipedia.org/wiki/Brute-force_search)的方法来解决这个问题？第一步是遍历数组中的每个元素。然后，我们必须检查每个选择的*元素和集合中剩余的*元素，测试它们的总和是否与目标值匹配。听起来像是一些嵌套循环的工作。**

这个解决方案在技术上确实有效，但是效率低得可怜。你必须为数组中的每个元素遍历整个数组，直到找到解！如果数组有三四个元素长，这听起来可能并不可怕。但是想象一下用一个一百万元素长的数组来做这件事。你必须数到一百万(技术上是 999，999)才能进入第二个元素！想想看，如果我们的解决方案是在数组的最后两个元素中找到的，那要花多长时间。

这个方法有一个很大的 O 符号 O(n ),这意味着随着数组的增长，寻找答案所需的时间呈指数增长*。一点都不理想。我们如何设计出更好的解决方案？*

# *实现两个指针*

*让我们再看一下问题的描述:*

> *给定一个整数数组(**已经按升序排序**)查找并返回两个元素的索引，当两个元素相加时，它们等于所提供的目标值。*

*我们如何利用这些信息来构建更有效的算法？有了这个知识，我们知道增加索引*总是*增加当前元素的值，减少索引将*总是*做相反的事情。*

*在前面的解决方案中，我们总是用第二个指针`j`递增索引，给定`i + 1`的值，然后遍历剩余的元素。相反，我们可以让`j`指向数组的最后一个元素*并添加一些条件逻辑。**

# *接近基本情况*

*我们的条件逻辑将需要识别我们的*基础用例*——退出我们函数的终止场景。对于我们当前的例子来说，这很简单。*

> *给定一个整数数组(已经按升序排序)，find 和**返回两个元素的索引，这两个元素加在一起等于提供的目标值**。*

*所以我们的基础案例是`array[i] + array[j] === target`。当条件满足时，我们可以退出函数，返回`[i, j]`作为解决问题的索引。我们如何从前面代码示例中的位置导航到那个基本条件呢？*

*有两种方法可以使我们不满足基本情况*的条件。**`***array[i]***`*`***array[j]***`***之和将大于或小于我们的目标值。结合我们所知道的递增和递减指数，我们得到了解决问题所需的一切。******

*****如果两个要素之和大于目标，减少其中一个指标将使我们更接近目标*** 。反之亦然。 ***如果两个要素之和小于目标值，增加其中一个指标将引导我们走向正确的方向。*** 我们应该选择哪个指标进行哪个操作？**

**我们的第一个指针`i`从 0 开始，所以很难再减少了(让我们暂时忽略负指数，因为它们与我们当前的问题没有任何关系或帮助)。类似地，我们的第二个指针`j`，从`array.length — 1`或者最后一个可能的索引开始。如果我们递增`j`，我们最终会指向一个不存在的元素！不太好。**

# ****看这两个指针****

**If you look closely you’ll notice an additional base case, which returns false when no two elements’ sum matches the target value.**

**查看我们完成的算法，你可以清楚地看到，它明显比暴力方法更有效。在最坏的情况下，该函数将迭代给定数组中的每个元素，从而使该函数的时间复杂度为 O(n)。这意味着我们改进的函数与 O(n)复杂度的强力方法相比要快得多。相当大的进步！**

# **概述**

**回顾一下，当在满足特定条件(即*基本情况*)的数组中搜索一对元素时，双指针技术是有用的。然后，我们评估这两个元素无法满足基本条件的方式，并编写条件逻辑，这将使我们更接近我们的目标。**

**双指针技术不是一种算法，而是一种*技术，*意味着有多种方法可以使用它。然而，基本原则是很重要的。使用两个(或更多)指针允许我们比暴力方法更有效地遍历和处理数据。这种技术在其他什么场景中可能有用？除此之外，您还可以如何编写上面提供的解决方案？(或许是以后博文的重点？)**

**如果你觉得这篇文章有帮助，请考虑订阅或与你的同事分享。非常感谢，祝编码愉快！**

```
**Resources:[**Two Sum II - Leetcode Challenge #167**](https://leetcode.com/problems/two-sum-ii-input-array-is-sorted/)*The inspiration for this write up*[**Two Pointers Technique - Geeks for Geeks**](https://www.geeksforgeeks.org/two-pointers-technique/)[**Grokking Algorithms: An Illustrated Guide for Programmers and Other Curious People**](https://bookshop.org/books/grokking-algorithms-an-illustrated-guide-for-programmers-and-other-curious-people/9781617292231)**
```