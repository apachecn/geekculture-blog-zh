# 我丢了 50L+包，因为我回答不了这个 JavaScript 问题。

> 原文：<https://medium.com/geekculture/i-lost-50l-package-because-i-couldnt-answer-this-javascript-question-2259bf90d021?source=collection_archive---------0----------------------->

这是一轮决定性的面试，有 3 个问题，我正确回答了第一个问题，第二个问题部分正确。现在，第三个问题是一个决定性因素，我无法回答它，因此我失去了一个非常好的包。我写了 50L，所以你会点击我的文章，但它真的是一个很好的包。

问题是什么？

"如何在 JS 中记忆带有可变参数的函数"

如果你懒得看文章，可以看看我下面的 Youtube 视频。如果没有，可以继续阅读。

![](img/210274b0ff56867a0713db71242e94cf.png)

> **问题解释:**

```
function add(num1,num2){
   return num1 + num2;
}function multiply(num1,num2, num3){
   return num1 * num2 * num3;
}console.log(add(10,20))
console.log(add(10,20))
console.log(add(10,20))
```

我们用相同的参数调用了 add 函数 3 次。我们可以记住 add 函数，并在第二次调用同一个函数时使用它，而不是计算 3 次加法。你写的记忆函数应该记忆所有的函数(例如:乘，加在上面的代码片段中)。

> **什么是记忆化**

对于那些不知道什么是记忆的人来说，这里有一个定义:

在计算中，记忆化是一种优化技术，主要用于加速计算机程序，方法是存储昂贵的函数调用的结果，并在相同的输入再次出现时返回缓存的结果

简单示例:

假设你要算 20！，计算完 19！不用计算 19！再乘以 20。你可以只存储 19 的结果！然后乘以 20。这样会节省很多时间和内存。

> **如何解决上述问题**

它有两个基本问题

1.  唯一标识函数及其参数的策略。
2.  写记忆功能

让我们一个一个地解决问题。

> **1。唯一标识函数及其参数的策略。**

最终，memoize 是一个具有键值对的对象。

例如:

{

【关键】:价值

}

为了记忆一个函数，我们需要为每个带有参数的函数调用找到一个唯一的 id。

下面是相同的代码。

```
function getUniqueId(fn, args){let uniqueId = [];uniqueId = uniqueId.concat(fn.name, args);return uniqueId.join("|");}
```

例如:如果你把函数和参数作为[11，22]传递

uniqueId 将是" **add|11|22** "我们使用" **|** "作为分隔符，因为我们需要唯一地标识像[1，222] [122，2]等参数，如果我们不使用" **|** "，那么所有这些值的键将是相同的。

> **2。写记忆功能**

非常简单，代码如下:

```
function memoize(fn){let cache = {}; return function(...args){ let uniqueId = getUniqueId(fn, args); if(cache[uniqueId]){ console.log('from cache'); return cache[uniqueId]; }else{ cache[uniqueId] = fn(...args); console.log('not from cache'); return cache[uniqueId] } }} let memoiseAdd = memoize(add);let memoiseMultiply = memoize(multiply);console.log(memoiseAdd(10,20));console.log(memoiseAdd(10,20)); console.log(memoiseMultiply(10,20,30));console.log(memoiseMultiply(10,20,30));
```

我们在上面所做的是，我们创建了一个名为 memoize 的函数，它将一个函数作为输入，并返回另一个函数，该函数将不同的参数作为输出。由于**闭包属性**，返回的函数将可以访问作为参数传递的函数。

之后就很简单了**memoisead**引用一个函数，你可以用它来调用 Add 函数任意次，只要参数匹配，它就会返回缓存的值。

示例缓存对象如下所示

```
{ 'add|10|20': 30 }
{ 'multiply|10|20|30': 6000 }
```

**以上方法有一定的局限性:**

1.  如果参数也包含“|”，那么管道不能作为分隔符。
2.  它不能像匿名函数那样正确地传递函数。

***这些可以用多种方法解决，我已经在我的 Youtube 视频中详细讨论过了。更多信息请观看。***

感谢您在下一篇文章中阅读 catch you。如果你还没有在媒体上关注我，那么请关注我，你可以在链接的[这里](https://www.linkedin.com/in/vasanth-bhat-4180909b/)上关注我。别忘了订阅我的 Youtube 频道[。](https://www.youtube.com/channel/UCSCNvSCk_Z9mBvUM-FJexRg/videos)

如果你想亲自和我讨论模拟面试，面试或简历审核的技巧和诀窍，你可以在这里预约:

[https://topmate.io/vasanth_bhat](https://topmate.io/vasanth_bhat)

如果你正在准备前端开发者面试，请观看我的以下系列:

如果你是 reactJS 开发人员，正在寻找面试准备，请点击这里观看我的完整面试准备指南: