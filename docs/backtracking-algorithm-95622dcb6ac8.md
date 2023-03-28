# 回溯算法

> 原文：<https://medium.com/geekculture/backtracking-algorithm-95622dcb6ac8?source=collection_archive---------9----------------------->

回溯是一种通用算法，可以用来寻找某些计算问题的一个或多个解。基本上，我们如何使用回溯，我们做出某种选择，如果选择是错误的，我们“回溯”到我们可以做出不同选择的地方，并从那里向前移动，直到我们得到正确的解决方案，或者在没有解决方案的情况下用完选项。

![](img/75e04951177562b593ca9c8461a81f1e.png)

Photo by [Daniel Irwin](https://unsplash.com/@metastudio35?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

由此，我们可以看到回溯经常与递归联系在一起，在递归中，我们递归地调用一个函数来遍历不同的选项，直到我们用一个有效解的标准或者如果我们没有找到一个解，来打破递归。回溯的一个很好的视觉例子是一个迷宫问题，你可以选择不同的路径，如果你选择的路径通向一个死胡同，而不是最终目标，你回溯到有不同路径可供选择的地方，并从那里向前移动，重复这个过程，直到你达到你的目标。对于这样的问题，我们还会跟踪你在迷宫中的位置，以防止无限的来回循环。回溯有很多用途，通常可以用作解决难题的算法，就像我之前提到的迷宫、数独或 n 皇后问题(这是一种将皇后放在棋盘上，使它们不能互相攻击的难题)。

回溯算法也不局限于谜题类型的问题。有几个问题我们可以使用回溯算法。为了展示这一点，我将使用回溯在 JavaScript 中编写 leetcode 的“组合和”问题的解决方案。这个问题接受一个数字数组和一个目标数字，我们的目标是返回一个数组，其中的数字加起来就是目标数字(我们可以重复数字)。我们首先需要创建两个数组我们的结果数组和一个临时数组，我们将添加和删除(回溯)直到我们得到一个正确的解决方案添加到我们的结果数组。然后我们创建我们的递归函数，它接受一个索引和一个目标数字作为参数。从那里开始添加条件语句，如果我们的目标低于 0，我们的递归函数就会中断，我们在临时数组中的总数太高了(不是一个解)，所以我们回溯并从临时数组中删除最后一项，然后添加原始数组中的下一个值。如果我们的索引等于原来的数组长度，我们就退出，因为没有更多的数字要检查。如果我们的目标等于零，我们就在临时数组中找到了一个有效的解，因为我们将当前数添加到临时数组中，并在每次递归时将它从目标中取出。我们从使用原始目标调用索引为 0 的递归函数开始。下面是编码后的解决方案，如果您了解递归是如何工作的，它会更容易理解:

![](img/da5522864a80242c819f1011370cb48b.png)

为了说明通过这个函数进行的回溯，这里列出了输入数组和目标数组分别为[2，3，6，7]和 7 时放入和取出的内容。

```
[ 2 ]
[ 2, 2 ]
[ 2, 2, 2 ]
[ 2, 2, 2, 2 ]      //We backtrack here to remove the last 2
[ 2, 2, 2, 3 ]      //We backtrack here to remove the 3
[ 2, 2, 2, 6 ]      //We backtrack here to remove the 6
[ 2, 2, 2, 7 ]      //We backtrack here to remove the 7 & 2
[ 2, 2, 3 ] //We push this as a solution & backtrack to remove the 3
[ 2, 2, 6 ]         //We backtrack here to remove the 6
[ 2, 2, 7 ]         //We backtrack here to remove the 7 & 2
[ 2, 3 ]     //The same pattern continues the until we reach the end
[ 2, 3, 3 ]
[ 2, 3, 6 ]
[ 2, 3, 7 ]
[ 2, 6 ]
[ 2, 7 ]
[ 3 ]
[ 3, 3 ]
[ 3, 3, 3 ]
[ 3, 3, 6 ]
[ 3, 3, 7 ]
[ 3, 6 ]
[ 3, 7 ]
[ 6 ]
[ 6, 6 ]
[ 6, 7 ]
[ 7 ]               //This is also as solution
```

这是递归回溯，解决一个问题，写出来，看看回溯发生在哪里，是很有用的。

这个 youtube 视频演示了我上面使用的 leetcode 问题，解释并用 Java[https://www.youtube.com/watch?v=yFfv03AE_vA](https://www.youtube.com/watch?v=yFfv03AE_vA)编写了一个回溯解决方案