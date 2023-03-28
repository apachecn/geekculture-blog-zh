# “跳跃游戏”软件工程问题

> 原文：<https://medium.com/geekculture/the-jump-game-software-engineering-problem-5f7b58d9e1ec?source=collection_archive---------14----------------------->

一个简单的，针对流行算法问题的优化解决方案。

![](img/570237e5faad741917b20f289bc36afc.png)

Photo by [Walker Fenton](https://unsplash.com/@walkerfenton?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 问题是

跳跃游戏可以在 leetcode [这里](https://leetcode.com/problems/jump-game/)找到。

给你一个整数数组。您从数组中的第一个元素开始，数组中的每个元素代表您可以“跳转”的最大索引数您需要确定是否有可能跳到数组的末尾。这里有一些例子。

在我的代码片段中，第一行是索引，第二行是实际的整数数组。当我试图深入理解一个涉及数组的算法问题时，这样方便且可见的索引对我帮助很大。从现在开始，我将把我的例子格式化成这样。

```
 0  1  2  3  4
[2, 3, 1, 1, 4]
```

对于这个例子，结果将是`true`。你可以这样跳:

```
 0  1  2  3  4
[2, 3, 1, 1, 4]
 ^ 0  1  2  3  4
[2, 3, 1, 1, 4]
       ^ 0  1  2  3  4
[2, 3, 1, 1, 4]
          ^ 0  1  2  3  4
[2, 3, 1, 1, 4]
             ^
```

或者像这样:

```
 0  1  2  3  4
[2, 3, 1, 1, 4]
 ^ 0  1  2  3  4
[2, 3, 1, 1, 4]
    ^ 0  1  2  3  4
[2, 3, 1, 1, 4]
             ^
```

这是另一个例子:

```
 0  1  2  3  4
[3, 2, 1, 0, 4]
```

对于这个例子，结果将是`false`。你将永远在索引 3 处结束。那里的值是 0，所以你被卡住了，在那个索引之前没有足够高的值可以跳过它。

## 解决方案

有可能用 O(n)时间和 O(1)内存解决这个问题。我们将使用一个[贪婪算法](https://en.wikipedia.org/wiki/Greedy_algorithm)，从数组的**末端** 开始。您可以从数组的开头开始，使用递归来测试每个可能的跳转路径。不过，从时间复杂性的角度来看，这并不太好。

在这里使用贪婪算法的后果是，我们可能找不到最佳的跳转路径，但这没关系。我们只需要确定是否存在跳转路径。如果任务是寻找最优的跳转路径，我会考虑使用递归，但是我还没有花太多时间考虑这个问题。

让我们首先创建一个保存数组最后一个索引的变量。然后，我们将从第二个到最后一个元素开始，到第一个元素结束，反向遍历数组。我的解决方案是打字稿。

```
function canJump(nums: number[]): boolean {
    let jump = nums.length - 1; for (let i = nums.length - 2; i >= 0; i--) { }
}
```

每次我们在 for 循环中迭代时，我们都要看看是否能从当前索引`i`到达我们存储为`jump`的最后一个索引。如果是这样，我们将把它存储为`jump`。

```
 0  1  2  3  4
[2, 3, 1, 1, 4]// i = 3
// jump = 4
// can you reach index 4 from index 3 if its value is 1?
// yes, so jump becomes 3// next in loop// i = 2
// jump = 3
// can you reach index 3 from index 2 if its value is 1?
// yes, so jump becomes 2// next in loop// i = 1
// jump = 2
// can you reach index 2 from index 1 if its value is 3?
// yes, so jump becomes 1// next in loop// i = 1
// jump = 2
// can you reach index 2 from index 1 if its value is 3?
// yes, so jump becomes 1// next in loop// i = 0
// jump = 1
// can you reach index 1 from index 0 if its value is 2?
// yes, so jump becomes 0// jump = 0
```

下面是我们的 TypeScript 解决方案中的情况:

```
function canJump(nums: number[]): boolean {
    let jump = nums.length - 1; for (let i = nums.length - 2; i >= 0; i--) {
        if (i + nums[i] >= jump) {
            jump = i;
        }
    }
}
```

那么，我们如何知道我们是否成功地跳过了整个数组，而不仅仅是数组的一部分呢？让我们考虑一个结果为`false`的例子。

```
 0  1  2  3  4
[3, 2, 1, 0, 4]// i = 3
// jump = 4
// can you reach index 4 from index 3 if its value is 0?
// no, so jump remains 4// next in loop// i = 2
// jump = 4
// can you reach index 4 from index 2 if its value is 1?
// no, so jump remains 4// next in loop// i = 1
// jump = 4
// can you reach index 4 from index 1 if its value is 2?
// no, so jump remains 4// next in loop// i = 1
// jump = 4
// can you reach index 4 from index 1 if its value is 2?
// no, so jump remains 4// next in loop// i = 0
// jump = 4
// can you reach index 4 from index 0 if its value is 3?
// no, so jump remains 4// jump = 4
```

我们知道，在 for 循环之后，当 jump 为 0 时，我们已经跳过了整个数组。因此，让我们将它添加到最终解决方案的代码中:

```
function canJump(nums: number[]): boolean {
    let jump = nums.length - 1; for (let i = nums.length - 2; i >= 0; i--) {
        if (i + nums[i] >= jump) {
            jump = i;
        }
    } return jump === 0;
}
```

我写这篇文章是因为我想提高自己对这个问题的认识。我希望我也帮助提高了你对它的理解。

感谢您的阅读！