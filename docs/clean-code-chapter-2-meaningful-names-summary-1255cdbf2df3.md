# 干净代码第 2 章—有意义的名称摘要

> 原文：<https://medium.com/geekculture/clean-code-chapter-2-meaningful-names-summary-1255cdbf2df3?source=collection_archive---------10----------------------->

## 用有意义的名字揭示代码意图

![](img/96b9b286477de77363cd7bf0a62b1646.png)

Photo by [Ed Robertson](https://unsplash.com/@eddrobertson?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

这是我个人对《干净的代码》这本书的阅读总结的一部分。

*   [清理代码第 1 章摘要](https://tekloon.medium.com/clean-code-chapter-1-reading-summary-4d9dcfb6067c)

在开始第二章之前，让我们问问你自己

> *有意义的名字对你来说意味着什么？*

*   有意义的名字指的是简单明了的东西吗？
*   有意义的名字是指那些你读完之后不会在你脑海中产生疑问的东西吗？

以下是书中最能引起我共鸣的有意义的名字的一些定义。

1.  透露意图
2.  避免心理映射
3.  使用名词作为类名&方法名应该有动词
4.  每个概念一个词

# 透露意图

一个有意义的名字应该能透露出意图。在本节中，我将提供 3 种不同的变量命名场景，

*   糟糕的变量命名
*   带有描述性注释的不良变量命名
*   良好清晰的变量命名
*   可发音的名字

让我们从变量命名开始。

## 糟糕的变量命名

```
const d = 24 * 60 * 60;
```

从上面的代码中，我们基本上不知道`d`对我们来说意味着什么？一段时间后，甚至代码作者也可能会忘记它。我们知道结果是`24 * 60 * 60`。但是我们肯定不知道它代表什么。因此，我们无法重复使用和维护它。

## 带有描述性注释的不良变量命名

然而，有些人可能会说，我们可以简单地将注释添加到代码中，这样可以消除误解，比如下面的代码。

```
const d = 24 * 60 * 60; // Day in seconds
```

然而，这仍然不是最理想的解决方案。因为每次当我需要使用变量`d`时，我都必须回到变量定义并检查注释。这将需要**上下文切换，并耗费我们的精神能量。**

## 良好清晰的变量命名

最理想的方法是使用一个恰当的变量名来揭示变量的意图。在这种情况下，变量被命名为`dayInSeconds.`

这很简单，我一眼就能知道`dayInSeconds`在做什么。

```
const dayInSeconds = 24 * 60 * 60;
```

这不仅适用于变量名，还应该适用于函数名、类名和每一段可能的代码，以保持我们的代码库干净。假设同一个函数有两个函数名选择，`getData`和`getTransactions`。

`getTransactions`在这里肯定更加清晰直白，而`getData`则给你留下了如`What kind of data`般充满想象的空间？

## 可发音的名字

通常，一个可发音的名字很容易透露出意图。如果你发不出这个音，你几乎无法和你的同事讨论。因此，一个可发音的名字总是更好地传达意图。

# 避免心理映射

这是我非常赞同的观点之一。很多时候，程序员倾向于使用需要脑力劳动的变量名。

书中提供的最差的变量命名是单字母变量名，如`a`和`b`。我们通常在 for 循环中使用它。如果逻辑保持简单，这通常在 for 循环中仍然有效。但是，**单个字母的变量名在大多数时候是最糟糕的选择。**

这是书中的一句名言。

> ***聪明的程序员和职业程序员的区别在于，职业程序员明白清晰才是王道。专业人士用他们的能力编写别人能理解的代码。—清理代码第二章***

上面的引用很好地提醒了我。当然，构建特性和编写代码是程序员的日常工作。但这还不够，我们还得专业一点。我们需要**用别人能理解的方式写代码**。

大多数项目通常有不止一个开发人员，所以以人们能够理解的方式编写代码将使生产力增长更快，更容易协作。否则，我相信随着团队的成长，这可能会降低生产力，因为现在每个人都在写只有他们自己理解的东西。团队成员现在不得不浪费时间去理解彼此的代码。

# 使用名词作为类名&方法名应该有动词

我们应该使用`noun`作为一个类名，比如`Customer`、`Product`和`Transaction`。而方法名中应该有动词，比如`updateAddress`。下面的简单例子是我们应该如何写我们的类名和方法名。

```
class Customerclass Customer {
  // Verb "update" in function name
  updateName(firstName, lastName)
}
```

# 每个概念一个词

在这里，每个概念用一个词是至关重要的。试着想象一下，如果你有两个角色相似的组件。但是，第一个组件被命名为`UserController`，而另一个组件被命名为`TransactionManager`。

如果你是团队的新成员，你会感到困惑。现在你面临困境，你开始质疑自己:

*   `Controller`和`Manager`有什么区别？
*   当它们在代码库中具有相同的角色时，为什么会有不同种类的概念名称？
*   最糟糕的是，你认为这可能是一些新的编程概念，并试图花时间搜索它们。**“控制者 vs 管理者”**。

所以，不要混淆自己和别人。每个概念选一个词，并一直坚持下去。

# 结论

对我来说，这一章中最重要的一点实际上是关于**如何以一种清晰直接的方式揭示你的代码的意图。**有意义的名字在这里起着最关键的作用。

然而，有意义的名字来之不易。有时，要花时间和几次重构才能得到它。所以在这个过程中要慢慢来。

## 简单可行的步骤

现在打开你的代码库，看看是否有任何变量或函数你可以重新命名，以便更清楚和理解。

# 通知；注意

这篇文章是我在 Gitbook 上写的阅读总结的一部分。你可以在这里阅读《T4》。

 [## GitBook

### GitBook

GitBookapp.gitbook.com](https://app.gitbook.com/s/JKemhZ8AuymtC4SZZOMf/chapter-2-meaningful-names) 

如果我错过了书中真正重要的东西，请随时给我建设性的反馈和建议。

我希望你喜欢它，并感谢阅读。