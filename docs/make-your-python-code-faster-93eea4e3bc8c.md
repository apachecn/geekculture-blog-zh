# 让您的 Python 代码更快

> 原文：<https://medium.com/geekculture/make-your-python-code-faster-93eea4e3bc8c?source=collection_archive---------1----------------------->

![](img/ed780eed0fe586ad66ae0e4f3bc73396.png)

Python is a slow language

你肯定听过很多人抱怨 Python 太慢了。我看到人们仅在性能方面将 Python 与 C 进行比较，但他们不会在快速开发的背景下进行比较。

它是一种动态类型语言，这意味着它的变量类型不是预定义的，尽管这是一把双刃剑，因为动态类型正是 Python 成为如此优雅的语言的原因。 ***所以 Python 是一种运行速度较慢，但打字速度较快的语言。***

让我们来看看一些小技巧，从长远来看，它们可能会对您的整体代码性能产生重大影响。

# 1.A，B = B，A

我敢肯定，您以前使用过“temp”作为占位符变量来交换两个元素。我能告诉你的是，方法属于课堂，而不应该在你编码的时候使用。

相反，选择一个简单的变量交换，按照***a，b = b，a*** 的顺序编写变量。这将在一行中切换所有变量，并防止解释器超过三行(temp，a，b 交换方法)。

> 这是一个微小的修正，可以节省几分之一秒的时间——但从长远来看，几分之一秒的时间会越积越多。

# 2.选择集合而不是列表

这是 Python 优化书籍中最古老的技巧之一。如果元素存在，不要在列表中搜索。相反，让列表成为一个集合(set(list))，然后检查“集合(list)中的元素”。这个微小的变化将提高您的运行时效率，因为集合中的 ***查找是 O(1)对列表中的 O(n)。***

> 然而，遍历集合并不比遍历列表快。

# **3。**列出理解

在 Python 中，一个很好的语法是 list comprehensions，它在创建列表时比一个循环在计算上更有效。因此，如果你需要，比方说，有一个前 10 个正整数的平方列表，而不是:

```
squares = []
for i in range (10):
    squares.append(i ** 2)
```

您可以:

```
squares = [i ** 2 for i in range(10)]
```

> 您可以尝试使用`[timeit](https://docs.python.org/3/library/timeit.html)`模块来比较哪种实现运行得更快。

你也可以像嵌套循环一样使用嵌套列表理解，但是通常不鼓励这样做，因为这样会使代码更难阅读、维护和测试。

# 4.在函数中导入

作为初学者，我们都喜欢在代码顶部大量导入我们认为需要的所有东西。我记得一次性导入 NumPy、Pandas、Math、Os 等，当我完成代码时，我只使用了三个库。这会耗尽计算机的内存。

相反，在相应的函数中导入所需的库(如果多个函数需要同一个库，则多次导入)。这意味着解释器只会在你调用函数的时候完成导入，而不是在代码开始的时候。

Python 库是缓存的，所以当你调用不同的函数时，每次导入都不会花费额外的时间。然而，当您最终在顶部导入所有内容，甚至不使用代码的某些功能时，它确实会花费更多的时间。

# 5.**了解内置函数**

当我开始使用 Python 时，我从不使用内置函数，所以为了完成我的绝对值代码，我会运行一个 for 循环，而不是使用 *abs()* 。为了将一个字符转换成大写，我甚至会将它转换成大写字母的 ASCII 等价物，因为我拒绝学习字符串函数。

如果你对 Python 很认真，实际上研究所有内置的 Python 函数是值得的，因为它不仅使你的代码更整洁和更可重用，你还可以通过简单地使用 Python 给你的东西来避免代码中微小的人为低效。

# **6。避免不必要的功能**

一个很好的例子是函数调用，这种方法可以帮助节省大量的运行时复杂性，但需要仔细考虑。虽然您确实希望函数提供良好的抽象性、可扩展性和可重用性，但是您可能不希望为每一件事情都提供一个函数，因为 Python 中的函数调用非常昂贵(如果您感兴趣，在本文[的](https://ilovesymposia.com/2015/12/10/the-cost-of-a-python-function-call/)中有一些有趣的观察)。

所以有时候，你可能想要牺牲，例如，写一个 getter 和/或 setter。另一方面，函数越少，代码的可测试性越差。因此，最终的决定实际上取决于您的具体应用。

# 7.避开圆点

如果您有一个对象，并且正在使用它的一些属性，请先将它们赋给局部变量:

```
rectangle_height = rectangle.height
rectangle_width = rectangle.width
```

所以如果你在代码的后面计算，比如说，它的表面，你会做:

```
surface = rectangle_height * rectangle_width
```

如果以后你也计算它的周长，你将重复使用相同的变量:

```
perimeter = 2 * rectangle_height + 2 * rectangle_width
```

出于同样的原因，通常不鼓励使用全局变量——你不希望浪费时间首先查找全局变量本身，然后查找你可能引用的它的每个属性。

> 这个博客到此为止。希望上面提到的所有小技巧能帮助你更好的优化代码。编码快乐！