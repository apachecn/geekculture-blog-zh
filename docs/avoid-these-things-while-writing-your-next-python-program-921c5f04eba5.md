# 在编写下一个 Python 程序时避免这些事情

> 原文：<https://medium.com/geekculture/avoid-these-things-while-writing-your-next-python-program-921c5f04eba5?source=collection_archive---------21----------------------->

![](img/cdd2e997ace013384c291cd5fdaeda78.png)

Photo by [Priscilla Du Preez](https://unsplash.com/@priscilladupreez?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果你已经用 Python 写了一段时间的代码，那么你一定见过 PEP 8 风格指南。如果没有，这篇文章应该会给你一些在编写下一个 python 代码时要记住的重要提示。

然而，并没有硬性规定你必须遵循所有的建议，当你有疑问时，你需要运用你的最佳判断。

## 1.刻痕

每个缩进层次使用 4 个空格。

空格优于制表符。当它已经用制表符缩进时，应该使用制表符。Python 摒弃了这两者的混合。

## 2.神秘的兰姆达斯

匿名函数帮助您创建易于阅读的内联 Python 代码，但是如果一行或一个表达式中有多个匿名函数，它们最终会变得混乱。

当在一行代码中间使用 lambda 时，80 个字符的规则很容易被违反。

Lambda 函数也不可重用，也不能被赋予一个有意义的名称来表达正在发生的事情。

一个不好的例子是这样的:

## 3.模糊的理解

我喜欢的 Python 最漂亮的特性是列表理解。但是，如果任何时候理解是嵌套的，那么理解正在发生的事情将是非常痛苦的:

如果理解是复杂的，那么把它放到一个单独的函数中是值得的。

## 4.80 字符规则

Python 代码的每一行应该最多包含 79 个字符，对于文档字符串或注释，行长度应该限制在 72 个字符以内。

此规则的主要目的是限制所需的编辑器窗口宽度，以便并排打开几个文件，这可能有助于代码审查。

虽然您可以选择更长的行长度，但是 Python 标准库是传统的，需要上述字符规则。

## 5.下注还是下注是个问题

根据 PEP 8，更好的方法是在二进制操作符之前中断，它遵循数学家和他们的出版商的方法来显示公式，从而产生更可读的代码。

## 6.尾随逗号

当创建一个元素的元组时，尾部逗号是必需的。要记住的模式是将每个值单独放在一行中，并在下一行添加右括号“)”。

## 结论

再次总结一下，这些指南来自 PEP 8 官方指南，但是没有任何硬性的规则可以遵循，你可以判断是否在下一个 Python 代码中使用它们。

## 参考

[](https://www.python.org/dev/peps/pep-0008/) [## PEP 8 风格的 Python 代码指南

### Python 编程语言的官方主页

www.python.org](https://www.python.org/dev/peps/pep-0008/) 

> 点击下面的链接查看更多这样的文章

[](/geekculture/10-python-snippets-that-you-should-use-every-day-6c1f076fd341) [## 你应该每天使用的 10 个 Python 片段

### №8 将使你成为专业的 python 程序员

medium.com](/geekculture/10-python-snippets-that-you-should-use-every-day-6c1f076fd341) [](/codex/5-advanced-automation-scripts-in-python-7dc0ff845099) [## 5 个高级 Python 脚本，像专业人员一样实现自动化！

### 这些都是生活窍门！

medium.com](/codex/5-advanced-automation-scripts-in-python-7dc0ff845099) [](/codex/5-advanced-code-snippets-for-your-python-programs-3377baa3d544) [## Python 程序的 5 个高级代码片段

### 使用它们，让人们惊奇

medium.com](/codex/5-advanced-code-snippets-for-your-python-programs-3377baa3d544) [](https://python.plainenglish.io/python-functions-that-make-life-easier-9d0c41b986a8) [## 让生活更简单的 5 个 Python 函数

### 让生活更简单的 Python 函数列表。

python .平原英语. io](https://python.plainenglish.io/python-functions-that-make-life-easier-9d0c41b986a8) 

你可以在我的个人资料中找到更多这样的内容。

[](/@rahulbhatt1899) [## 拉胡尔·布哈特—中等

### 阅读媒体上的拉胡尔·布哈特作品。我写 Python 脚本和文章。GitHub…

medium.com](/@rahulbhatt1899)