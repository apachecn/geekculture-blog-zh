# JavaScript:比较 Map()和 forEach()数组方法

> 原文：<https://medium.com/geekculture/javascript-comparing-map-and-foreach-array-methods-b6e061c3804b?source=collection_archive---------7----------------------->

![](img/ad395bf8120d5dd99591dd35d1f40212.png)

Photo by [Jason Dent](https://unsplash.com/@jdent?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

容易混淆的两种数组方法是`map()`和`forEach()`。跟踪这两者之间的差异可能有点模糊——但是每一个都有独特的优势，可以使它们成为某些情况下的理想选择！

当你选择一个数组方法时，首先要考虑的是在它执行后你需要返回什么。你需要一个新的阵列吗？你只想要单品吗？什么能让你做下一步需要做的事情？

`map()`和`forEach()`都迭代一个现有的数组，并且都为数组中的每一项执行一次所提供的函数。但是他们更像异卵双胞胎而不是同卵双胞胎！

## 句法

下面是`map()`的语法:

```
const localFruits = ["apple", "pear", "strawberry"];const mapFruit = localFruits.map(fruit => console.log(fruit));
```

下面是`forEach()`的语法:

```
const fancyFruits = ["papaya", "dragonfruit", "mango"];const eachFruit = fancyFruits.forEach(fruit => console.log(fruit));
```

是的，鹰眼，语法遵循相同的模式！两者都接受一个表示当前数组项目的参数——在本例中，我从水果数组中将其命名为`fruit`。我可以称它为`x`或`item`，但是需要有一个参数传入。

## 返回值

*   `map()`遍历原始数组，返回一个全新的数组。
*   `forEach()`返回`undefined`。永远永远。

如果它不返回任何东西，你为什么要使用`forEach()`？嗯，`forEach()`非常适合*做*的事情——比如将项目打印到屏幕上或者将项目添加到列表中。我认为它是数组方法中的玛丽·近藤(Marie Kondo)——它接触项目一次，执行所需的操作，然后很快就忘记了。用`forEach()`内存什么都不存，所以不会产生不必要的内存压力；`map()`分配内存并存储返回值。

## 可链接性

在一行连续的代码中一个接一个地调用方法被称为*链接*，它可以让你的代码更高效、可读性更强。(有一个阈值，在这个阈值上这是不真实的，循环而不是链接是一个更好的选择，但一般来说，如果没有人太得意忘形，链接是一件好事。)

*   `map()`是可链接的，可以将其新数组传递给另一个链接的方法。
*   `forEach()`不可链接。它在内存中什么都没有，所以没有什么可以传递给下一个方法。

## 易变性

在你的方法执行之后，原始数组会发生什么？

*   `map()`不变异原数组；它创建一个新的数组，并保留原来的数组。
*   `forEach()`不变异原数组；它对每个数组项执行所请求的操作，而不处理原始的。

## 真的吗？

两者的根本区别在于，`map()`返回一个值数组，而`forEach()`返回 undefined。因为`map()`提供了一个回报，你可以把它链接起来(你不能用`forEach()`做到这一点)。哪一个是最好用的将取决于你想要的输出！