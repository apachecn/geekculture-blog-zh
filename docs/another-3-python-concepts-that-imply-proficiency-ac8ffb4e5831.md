# 暗示熟练的另外 3 个 Python 概念

> 原文：<https://medium.com/geekculture/another-3-python-concepts-that-imply-proficiency-ac8ffb4e5831?source=collection_archive---------14----------------------->

![](img/b0ed8ba2e589d6b5a65693f5c65443fb.png)

Magnifying glass

在[之前的](/geekculture/3-simple-python-concepts-that-will-advance-your-career-9ed18cf08f5c)文章中，我们仔细研究了一些 python 概念，这些概念证明了该语言的专业知识，并将帮助你在职业生涯中取得进步。我得到的反馈让我意识到，许多人认为它很有用，这导致了你正在阅读的文章。在这里，我们将讨论另外 3 个 python 概念，它们展示了对它的熟练程度。

## 💭（听力或阅读）理解测试

理解是一种从其他序列创建序列的快速而简洁的方法。作为一个序列，我们考虑任何可以迭代的东西。这包括列表、字典、集合、生成器等。这听起来可能有些模糊，但是通过一个例子，一切都会变得更加清晰。假设我们有一个数字列表，我们想创建另一个包含原始列表中数字的 2 次方的列表。传统的做法是:

```
my_list = [1, 2, 3, 4, 5] 
pow_of_2 = [] 
for number in my_list: 
    pow_of_2.append(number * number)
```

我们有通常的 for 循环，我们迭代原始列表，并且每次都追加新计算的数字。理解的方法如下:

```
pow_of_2 = [number * number for number in my_list] 
```

如你所见，使用理解类似于说话。如果我们要阅读上面的表达式，它将是“创建一个包含序列中每个数字的次数本身的列表”。这不仅适用于列表，也适用于任何序列。例如字典:

```
pow_of_2_dict = { number: number * number for number in my_list }
```

有什么好处？对于初学者来说，使用理解是很有意义的。你可以很容易地理解它做什么，而不需要通读代码行。此外，有人可能认为理解比传统循环更快，但这是另一个话题(如果你感兴趣，[这个](https://stackoverflow.com/questions/22108488/are-list-comprehensions-and-functional-functions-faster-than-for-loops) SO 答案给你一个很好的概述)

## 📦拆包

解包是 Python 中的一项操作，包括在一条赋值语句中将一系列值赋给 iterable。在早期，使用的序列是元组，但是许多 Python 开发人员一直在使用它，并且它已经被推广到许多序列。上面的内容可能一开始听起来有点混乱，但是一个例子会让你明白。

让我们想象一下，我们有一个函数来寻找整数列表中的最小值和最大值。这个函数不止做一件事，所以你应该不会在野外遇到。我们在这里只是把它作为一个例子:

```
def find_min_max():
    return min(my_list), max(my_list)
```

这可能没有多大意义(返回 2 个参数)，但在 Python 中，您可以使用以下表达式调用此函数:

```
list_min, list_max = find_min_max()
```

现在，这两个变量包含了需要的值，赋值在一行中完成。这个小技巧让我们的代码更容易阅读、维护和扩展。例如，如果我们不需要最小值，我们可以使用一次性变量。但是这可能是下一篇文章的内容。

在这个工具中，我们需要非常小心的是变量匹配。返回值必须与等待在表达式另一端的值数量相同。如果不是这样，Python 会抛出一个`ValueError`。

**奖励:**函数中引用了一个列表，但是我们没有传递任何参数。好奇想知道为什么吗？点击[这里](/geekculture/3-simple-python-concepts-that-will-advance-your-career-9ed18cf08f5c)！

## 📋数据类

在标准 Python 3.7 版本中引入的`dataclass`是一个用于创建类的模块，主要用于存储和操作类中的数据。它附带了一组函数，使得处理数据类更加容易。当然，可以在早期 Python 版本中使用 dataclasses，但是需要手动安装。

使用`dataclass`模块声明类的方法是利用*装饰器*:

```
from dataclasses import dataclass
from typing import Optional@dataclass
def Car:
    plate_number: str
    colour: Optional[str] = None
```

这就是我们需要做的。现在我们可以使用这个类来创建对象:

```
my_car = Car("ABC 1234")
```

有人可能会问，*“_ _ init _ _()方法在哪里？”。*`dataclass`确保将相应的参数分配给正确的变量。它还负责数据类型和默认值。

初始化对象后，您可以轻松访问其属性:

```
>>> my_car.plate_number
"ABC 1234"
```

除了`__init__()`方法外，`__eq__()`和`__repr__()`方法的工作原理也不同。

`__eq__()`方法用于检查对象之间的相等性。默认情况下，不同的对象存储在不同的内存空间中，因此当比较同一类的对象时，该方法将返回`False`，即使它们持有相同的数据。但是对于数据类来说，情况并非如此。因为它们被用来存储数据，如果对象中的所有数据都相等，这个方法返回`True`。

`__repr__()`方法用于处理打印类对象时显示的内容。在常规类中，如果我们打印一个实例，我们通常会得到类似于`<Class.Class object at 0x868df81>`的结果。有了数据类，我们得到了更有说服力的信息。在上面的例子中，我们会得到:

```
>>> print(my_car)
Car(plate_number="ABC 1234", colour=None)
```

正如我在上一篇文章中所述，这只是 Python 所能提供的一瞥。如果你真的想掌握它，你将需要投入时间，当然，你会得到回报。