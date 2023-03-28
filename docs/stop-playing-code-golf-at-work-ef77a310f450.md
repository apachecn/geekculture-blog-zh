# 不要在工作时玩代码高尔夫

> 原文：<https://medium.com/geekculture/stop-playing-code-golf-at-work-ef77a310f450?source=collection_archive---------9----------------------->

较短的代码并不是更好的代码。

![](img/16c719019e723c10d8688570abe7a736.png)

Golf Club and Golf Ball

# 什么是代码高尔夫？

来自维基百科，

> **代码高尔夫**是一种娱乐性的[计算机编程](https://en.wikipedia.org/wiki/Computer_programming) [竞赛](https://en.wikipedia.org/wiki/Competition)，参与者努力在最短的时间内获得解决某个问题的[源代码](https://en.wikipedia.org/wiki/Source_code)。

娱乐？那很好——都很有趣。但是，当你在专业的生产环境中看到它时，它就会成为一个问题。

# 为什么不好？

假设您刚刚被要求向现有项目添加一个特性。

你看的大多是别人的代码。他们可能忙于自己的工作，没有时间坐下来和你聊一聊。他们可能已经离开了你的组织。它甚至可能是你自己的代码，但是你已经很久没有接触过它了。

您正在查看一个需要更改的 TypeScript 函数:

```
function GetNumberCategory(a: number): number {
    return a < 0 ? a%2 === 0 ? 3 : 1 : a%2 === 0 ? 4 : 2;
}
```

你需要一分钟来理解它到底在做什么。**代码可读性差，可扩展性差**。`a`是什么意思？返回的数字代表什么？也许你是一名实习生，或者是一名较新的程序员，你以前从未遇到过三元运算符。

对于一个刚接触这个项目的开发人员(或者在很长一段时间后回到这个项目的开发人员)来说，理解这个函数并对它进行修改会容易得多，如果它看起来像这样的话:

```
function GetNumberCategory(somethingId: number): number {
    let isEven = somethingId%2 === 0; // 0 is considered positive because [business requirement]
    let isPositive = somethingId >= 0; if (!isEven && !isPositive) {
        return 1; // odd and negative
    } if (!isEven && isPositive) {
        return 2; // odd and positive
    } if (isEven && !isPositive) {
        return 3; // even and negative
    } if (isEven && isPositive) {
        return 4; // even and positive
    }
}
```

这个函数完成同样的事情需要更长的时间。但是绝对值得。很容易看出，最初理解和修改更容易。

> 对于另一个开发人员来说，理解这段代码并在将来对其进行修改有多简单？

**在你写代码的时候，你应该首先想到**。

在这种特定的情况下，没有明显的或有关的性能损失。只是[句法糖](https://en.wikipedia.org/wiki/Syntactic_sugar#:~:text=In%20computer%20science%2C%20syntactic%20sugar,style%20that%20some%20may%20prefer.)。

我建议你不要在可读性和可伸缩性方面走得太远，以至于牺牲了性能。是的，对有序数组的线性搜索比二分搜索法更容易理解，但是性能差异太大了。

# 结论

编写可读性和可伸缩性的代码是重中之重，但是不要为了可读性和可伸缩性而牺牲性能。

只是为了好玩——这是来自 Code Golf 维基百科页面的 GolfScript 代码片段:

```
;''
6666,-2%{2+.2/@*\/10.3??2*+}*
`1000<~\;
```

你能猜出它是做什么的吗？