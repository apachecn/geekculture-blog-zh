# 解决 Meta，亚马逊，谷歌，苹果，微软前端开发者面试问题

> 原文：<https://medium.com/geekculture/solve-meta-amazon-google-apple-microsoft-frontend-developer-interview-question-f492eed948c7?source=collection_archive---------5----------------------->

在我在网上做了很多研究后，我得出一个结论，没有专门为前端开发者准备的教程/视频系列。所以，我决定在我的 [Youtube 频道](https://www.youtube.com/results?search_query=uncommon+geeks)中解码 MAANG 最常见的面试问题。在这篇文章中我将讨论一个非常有趣的问题。所以，一直读到最后。

![](img/3e5ee95004b6be54756b5cae42ac24f9.png)

Flatten an array without using Recursion.

> **问题:**展平给定数组

**样本输入:**【1，2，[3，4]，5】

**样本输出:** [1，2，3，4，5]

问问题的公司**:Meta/微软/谷歌/苹果/亚马逊**

我已经写了这个问题的递归解决方案[这里](https://mevasanth.medium.com/flatten-array-of-array-in-javascript-microsoft-interview-question-345c71ff9ccd)。了解递归方法非常重要，因为大多数公司都希望你了解这个解决方案。但是迭代方法也不是那么容易。因此，在面试中，他们可能会要求你写下两种解决方案。因此，在这篇文章中，我将解释迭代方法。

> **逻辑的症结**

```
let arr = [1,2,[3,4]];
let last = arr.pop(); // Ex: [3,4]
arr.push(...last); // Spread operator will convert [3,4] to 3,4
console.log(arr)
```

复制这段代码并运行，您应该可以洞察到逻辑的症结所在。下面详细解释一下。

**算法**

1.  取嵌套数组的最后一个元素，
2.  如果它也是一个数组，那么我们使用 spread 运算符来消除数组，并将其推回数组。
3.  如果它不是一个数组，那么不要使用 spread 运算符，只需将值推入数组。

> **完整代码**

```
function flatten(input) {
  const stack = [...input];
  const res = [];
  while (stack.length) {
    // pop value from stack
    const next = stack.pop();
    if (Array.isArray(next)) {
      // push back array items, won't modify the original input
      stack.push(...next);
    } else {
      res.push(next);
    }
  }
  // reverse to restore input order
  return res.reverse();
}

const arr = [1, 2, [3, 4, [5, 6]]];
flatten(arr);
```

**说明:**

```
const stack = [...input]; 
// it is just a deep copy of variable. Otherwise source array will // be modified
```

**res** 变量用于保存最终结果。

返回值是 reversed res，因为我们从最后一个开始执行我们的活动。因此，在最终发送结果时，应该将结果颠倒过来，使其符合实际顺序。

```
return res.reverse();
```

如果你没有在媒体上跟随我，请跟随。别忘了订阅我的 Youtube 频道，

如果你想亲自和我讨论模拟面试，面试或简历审核的技巧和诀窍，你可以在这里预约:

[](https://topmate.io/vasanth_bhat) [## 在 topmate.io 上预订与瓦桑特的时间

### 为世界顶尖人才简化个性化互动

topmate.io](https://topmate.io/vasanth_bhat) 

如果你正在准备前端开发人员面试，请观看我的以下系列:

[https://www.youtube.com/watch?v=qcixpy3HQ9s&list = PLM cro 0 zwqv 4 qmslgjqg 7n 8 azahkc 5 pj 4t](https://www.youtube.com/watch?v=qcixpy3HQ9s&list=PLmcRO0ZwQv4QMslGJQg7N8AzaHkC5pJ4t)

如果你想学习内置方法的 JavaScript 自定义实现，那么看下面我的系列:

[https://www.youtube.com/watch?v=eGzErMUfdpk&list = PLM cro 0 zwqv 4 rwqjzcajakbadcjjmkal _ I](https://www.youtube.com/watch?v=eGzErMUfdpk&list=PLmcRO0ZwQv4RWqjZCajaKBAdCJjmkAL_I)