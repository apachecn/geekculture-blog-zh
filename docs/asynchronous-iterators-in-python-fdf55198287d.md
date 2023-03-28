# python 中的异步迭代器

> 原文：<https://medium.com/geekculture/asynchronous-iterators-in-python-fdf55198287d?source=collection_archive---------1----------------------->

## 如何用 python 编写异步 for 循环

![](img/357cc71164791585472f3abe84c0e005.png)

[https://dock2learn.com/tech/asynchronous-iterators-in-python/](https://dock2learn.com/tech/asynchronous-iterators-in-python/)

# 简介:

作为一名开发人员，我们现在都和`python`的`iterators`和`iterables`一起工作过。这篇文章不是对`python`中迭代器和可迭代对象的另一个区别或比较。让我把多余的东西拿出来。这篇文章概述了`asynchronous iterators`如何工作以及如何编写我们自己的`asynchronous iterators`。

要了解`asyncio`的介绍，请参考[这里的](https://dock2learn.com/tech/asyncio-in-python/)。

# 带走:

在这篇文章的结尾，你应该能够

*   写一个异步迭代器。
*   了解`async`迭代器的内部。
*   正规`iterators`介绍。
*   非异步迭代器与异步迭代器有何不同。

现在，让我们开始吧。我们走吧。

# python 中的迭代器:

在讨论`async`迭代器之前，让我们快速浏览一下 python 中的常规迭代器。

*   迭代器必须实现`__iter__`特殊方法。
*   `__iter__`特殊方法应该返回一个`iterable`，即任何实现`__next__`特殊方法的对象。这可能是它自己的类(`self`)或任何其他类对象。
*   `__next__`方法具有运行`iterator`的逻辑，直到满足一个条件。
*   一旦条件满足，应出现`StopIteration`错误信息。

现在，让我们快速编写一个`iterator`，`OddCounter`，它只迭代奇数。我知道这并没有什么意义。但是玩玩`iterators`的能力还是很有趣的。你可以尝试实现一个偶数计数器或者斐波那契计数器。

输出:

```
1
3
5
7
9
...
...
93
95
97
99
```

上面的例子只是为了说明`python`中常规`iterators`的工作原理。既然我们已经学会了，让我们跳到我们在这里的目的。

# 异步迭代器:

在常规迭代器中，如果有一个场景，您必须执行一些 I/O 任务来获取`__next__()`方法中的数据，该怎么办？这正是`async iterators`要做的。

`async iterator`通常包含:

*   一个`__aiter__()`方法而不是`__iter__()`方法。
*   `__aiter()__`方法必须返回一个实现了`async def __anext__()`的对象。
*   `__anext__()`方法必须为每次迭代返回一个值，并在结束时抛出`StopAsyncIteration`，而不是`StopIteration`。

现在，让我们尝试使用异步迭代器重写我们的`OddCounter`迭代器。

输出:

```
1
3
5
7
9
...
...
93
95
97
99
```

虽然这在语法上是正确的，并且有效，但是我们没有任何令人信服的理由在这里使用`async iterator`。一个这样的场景是，如果我们在`__anext__()`方法中做一些需要从其他地方获取数据的操作。例如，如果我们一次从一个远程数据库或另一个协程函数获取一个数据，那么它将是编写异步迭代器的最佳选择。

在上面的例子中，`KeyTaker`是一个异步迭代器，它从`coroutine`中提取键。现在你可能会说这可以用一个简单的类和函数来完成。我们当然可以。这里的要点是，`all_keys()`方法可以执行 I/O 操作，比如从数据库获取信息或调用 API。为了简单起见，我刚刚创建了一个字典，并为查询的键返回它们的值。

`main()`函数只是遍历`KeyTaker`并打印值。我们也可以在`async for`循环中调用另一个协程。这将使代码完整。

# 总结:

*   异步迭代器有 __aiter__ 和 __anext__ 特殊方法
*   StopAsyncIteration 应在迭代结束时引发。
*   如果我们按需执行一个 I/O 操作，而迭代器又在执行另一个 I/O 操作，那么可以使用异步迭代器。

# 参考资料:

[https://peps.python.org/pep-0525/](https://peps.python.org/pep-0525/)

*原载于 2022 年 3 月 10 日 https://dock2learn.com**T21*[。](https://dock2learn.com/tech/asynchronous-iterators-in-python/)