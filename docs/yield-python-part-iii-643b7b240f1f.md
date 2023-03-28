# yield — Python(第三部分)

> 原文：<https://medium.com/geekculture/yield-python-part-iii-643b7b240f1f?source=collection_archive---------5----------------------->

![](img/5d58a622d730b12adc12f7034795fe05.png)

Photo by [Annie Spratt](https://unsplash.com/@anniespratt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/compost?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在本文中，我们将看到如何关闭生成器并向生成器抛出异常。如果你想知道什么是生成器，发送值给生成器，你可以分别看`yield`系列的[第一部分](https://dkhambu.medium.com/yield-python-part-i-4dbfe914ad2d)和[第二部分](https://dkhambu.medium.com/yield-python-part-ii-e93abb619a16)。

让我们举一个使用生成器读取文件的例子:

generator with call to close() example

输出是:

```
state after initialization: GEN_CREATED
**Sonnet 65: Since brass, nor stone, nor earth, nor boundless sea**
By William ShakespeareSince brass, nor stone, nor earth, nor boundless sea
But sad mortality o'er-sways their power,state after iterating over 5 lines: GEN_SUSPENDED
closing generator with .close() ...
state after closing the generator: GEN_CLOSED
```

我们看到我们可以使用`.close()`方法关闭发电机。因此，`.close()`方法后发电机的状态为`GEN_CLOSED`。`GeneratorExit`幕后，异常发生器被调用。

behind the scene of `close()` call in a generator

输出是:

```
first value: a
closing generator...
Generator close() is called...
cleanup here...
```

调用`close()`时 Python 的期望:

**i)** 一个`GeneratorExit`异常冒了出来- >这个异常被 Python 沉默了

我们在示例 1 中看到，我们不需要捕获`GeneratorExit`异常。它由 Python 负责。

**ii)** 发电机干净地存在(返回)——>对呼叫者来说，一切工作“正常”

generator closing without exception example

输出是:

```
print all values : [1, 2]
state of generator after getting all value: GEN_CLOSED
```

看，在我们通过`list`调用发电机的所有值后，发电机的状态是`GEN_CLOSED`。

**iii)** 发生器内部出现其他异常- >调用者看到异常

catching exception that is raised from inside the generator

运行时，输出为:

```
z
StopIteration Encountered.
GEN_CLOSED
```

看，当我们试图从耗尽的生成器中获取下一个值时，我们得到了在调用者中捕获的`StopIteration`异常。发生`StopIteration`异常后，发电机关闭。

我们还可以使用`throw()`方法向生成器发送异常。

throw() example in generator

输出是:

```
received: helloValueError exception encountered.
GEN_SUSPENDEDreceived: worldStopIteration exception encountered.
StopIteration exception caught by a caller
GEN_CLOSED
```

看到我们可以`throw()`一个生成器的异常，并且可以在一个生成器中处理它们。如果一个异常不是由生成器处理的，那么这个异常将被冒泡到调用者那里。

在本例中，我们向生成器抛出了`StopIteration`异常。如果我们有一个来自生成器的 **return** 语句，在`StopIteration`被处理后，调用方将会收到`StopIteration`异常，我们已经在调用方处理过了，生成器的状态得到**关闭**。

如果一个异常被抛出到一个发生器，并且在发生器中被捕获，但是**没有** **引发**任何异常或者**返回**，如示例中抛出的`ValueError`异常所示，发生器的状态将处于**暂停**。

如果我们从任何类型的异常处理中返回，调用者都会看到`StopIteration`,如下例所示:

输出是:

```
received: hello worldValueError exception encountered.
StopIteration exception caught by a caller
GEN_CLOSED
```

总之，有一种`close()`方法可以退出发电机并将发电机置于`GEN_CLOSED`状态。这是由 python 通过处理`GeneratorExit`异常来完成的，不需要调用者的参与。调用方可以看到生成器引发的任何其他类型的异常。我们也可以向生成器抛出异常。如果生成器返回或引发异常，调用者将会看到它。

本文到此为止。我希望这是一个有用的！我的下一篇文章会在`yield from`上！

感谢您的阅读，再见。

灵感:

*   [蟒蛇深潜](https://www.udemy.com/course/python-3-deep-dive-part-2/)

你可以在 [Patreon](https://www.patreon.com/dkhambu) 上支持我！