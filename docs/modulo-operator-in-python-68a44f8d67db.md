# Python 中的模运算符

> 原文：<https://medium.com/geekculture/modulo-operator-in-python-68a44f8d67db?source=collection_archive---------13----------------------->

![](img/222e9d21fb51b594fae2436e08012cbd.png)

Photo by [Lisanto 李奕良](https://unsplash.com/@lisanto_?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/paper-windmill?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

已经一个多月了，我还没有写新的心得。所以在这里我要写的是 Python 中的模运算符。

被负数的模运算符搞糊涂了。我曾经遇到过一次。所以我在这里列出了模运算的方式。

模数快速入门:

模项来自数学的一个分支，叫做**模运算。**模运算符用于获取有限边界内的值。一个经典的例子是时钟。十二小时制的时钟有一组固定的值，从 1 到 12。它以 12 为模。

如果我们想知道上午 8 点 7 小时后是什么时间，你可以简单地将 7 加 8，然后取它的模。

```
8 o'clock + 7 = 15
15 mod 12 = 3
```

这意味着上午 8 点过 7 小时是下午 3 点

现在你可以认为 15 和 3 在 mod 12 上下文中是等价的。模运算将这种关系描述为

```
a ≡ b(mod n)
```

在我们的例子中:

```
15 ≡ 3 (mod 12)
```

读作 15 等于 3 模 12。

现在让我们找出不同类型数的模。

对于正被除数和正除数:

```
>>> 15%2
1
```

对于负被除数和正除数:

```
>>> -15%2
1
```

我们看到，对于除数为正的正负红利，余数都为正。

现在让我们来看看正被除数和负除数:

```
>>> 15%-2
-1
```

对于负被除数和负除数:

```
>>> -15%-2
-1
```

我们看到，对于除数为负的正负红利，余数为负。

Python 有一个叫做`divmod`的方法，它给了我们被除数和余数。让我们看看在上述情况下我们得到了什么值:

```
>>> divmod(15,2)
(7, 1)>>> divmod(-15,2)
(-8, 1)>>> divmod(15,-2)
(-8, -1)>>> divmod(-15,-2)
(7, -1)
```

我们看到，对于正除数，模为正，对于负除数，余数为负。股息会相应调整。

然而，根据您获取余数的方式，此规则不适用于浮点数的模运算符:

```
>>> math.fmod(-12.0, 5.0)
-2.0>>> -12.0%5.0
3.0
```

如果我们使用`math.fmod`得到余数，即使除数是正的，余数也是负的。而如果我们用正规的取模方法，我们会得到正余数。

这是为什么呢？这是因为语言是如何得到商的——`trucated division`或`floor divison`。

对于截断除法，负商四舍五入为零，使用以下公式:

`r = a — (n * trunc(a/n))`

在哪里

```
r = remainder
a = dividend
n = divisor
```

`math.fmod`使用这个`truncated division`规则，因此即使除数为正，当被除数为负时，我们得到负余数。

让我们详细说明这个等式:

```
r = -12.0 - (-5.0 * trunc(-12.0/5.0))
r = -12.0 - (-5.0 * trunc(-2.4))
r = -12.0 - (5.0 * -2)
r = -12.0 + 10
r = -2
```

对于下限除法，负商从零四舍五入，使用以下公式:

```
r = a - (n * floor(a/n))
```

在哪里

```
r = remainder
a = dividend
n = divisor
```

简单模使用这个`floor division`，因此当被除数为负，除数为正时，我们得到正余数。

```
r = -12.0 - (5.0 * floor(-12.0/5.0))
r = -12.0 - (5.0 * floor(-2.4))
r = -12.0 - (5.0 * -3)
r = -12.0 +15
r = 3
```

就是这样。

总之，在 Python 中，对于整数值，如果除数为正，余数将为正，否则为负。对于负浮息，取决于你如何得到模值。如果使用普通的`%`运算符，由于使用了`floor`除法，余数将为正。如果你使用`math.fmod`，你将得到负的余数，因为它将使用`truncated`除法。

希望对你有帮助。感谢您的阅读。🪁

灵感:

*   [真正的蟒蛇](https://realpython.com/python-modulo-operator/)

你可以在第七天支持我。