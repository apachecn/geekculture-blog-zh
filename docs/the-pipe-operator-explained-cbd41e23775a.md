# 管道操作员解释道

> 原文：<https://medium.com/geekculture/the-pipe-operator-explained-cbd41e23775a?source=collection_archive---------7----------------------->

## 了解如何改善您的脚本编写体验

![](img/523b00c1705b8c2e22fd826ab5e329cd.png)

Photo by [Sigmund](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

计算机语言有许多运算符，可以用来对变量执行操作。所有计算机语言中都有标准的代数运算符。然而，在 shell 脚本中，有一个独特的操作符叫做管道操作符。很多初学 shell 脚本的人听说过这个操作符，但是在他们创建脚本的时候从来没有想过使用它。

下面是管道操作员的解释。

## 什么是管道操作员？

管道运算符是一个接口，它允许您从一个命令获取标准输出，并将其输入到另一个命令的标准输入中。

## 管道操作符有哪些操作系统外壳脚本？

Unix 操作系统的目标之一是软件的模块化。以前，程序是单一的，每个人用不同的编程语言在不同的软件上编写相同的实现。然后人们开始怀疑程序是否可以重用相同的实现。最终，图书馆被创建来解决这个问题。然后，人们开始问，进一步的模块化是否会发生在应用程序级别。所以 Unix 的创建者将管道添加到脚本特性集中，以实现程序的进一步模块化。

shell 脚本的进一步迭代继续保持这一特性。Unix shells 演变成了 Linux 和 BSD shells。而 Windows 也复制了在他们的批处理脚本语言中运行的 Unix 管道。

## 编程语言有管道吗？

管道概念不仅仅是一个脚本特性。它们也存在于编程语言中。例如，在 Java 中，你有[流 API](https://docs.oracle.com/javase/8/docs/api/java/util/stream/Stream.html) 。API 允许您在功能上操作数组的状态。您可以使用 reduce 和 filter 命令来减少存储在数组中的信息量。或者使用 map 命令增加数组中存储的信息量。

几乎每一种现代流行语言都有一个功能接口，允许你传递信息。这里有一些语言，但不限于，有类似的管道功能。

*   [C++](/swlh/doing-it-the-functional-way-in-c-5c392bbdd46a)
*   [Java](https://www.geeksforgeeks.org/functional-interfaces-java/)
*   哈斯克尔
*   等等

## 如何使用管道？

```
command_0 | command_1 | ... | command_n
```

你正在为信息的流动创造一个管道。随着您添加更多命令，您可以添加更多信息或减少信息量。最后，您会得到满足预期条件的最终文本产品。

## 你为什么想用管子？

**更容易调试**

![](img/048dc038d2b3c89eaa090eb3f9935c27.png)

Photo by [Clément Hélardot](https://unsplash.com/@clemhlrdt?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

[在命令式编程中，你会有很多动作可能修改同一个状态。这使得很难确定是哪种行为导致了州政府的问题。](https://www.ionos.com/digitalguide/websites/web-development/imperative-programming/) [然而，在声明式编程中，你会有许多动作会有相应的状态。您将知道哪个操作会导致不正确的状态更改，从而可以做出相应的更改来解决问题。](https://www.ionos.com/digitalguide/websites/web-development/declarative-programming/)

**更容易阅读**

![](img/80791902c8f406c7b453fc508395ce41.png)

Photo by [Heather Ford](https://unsplash.com/@the_modern_life_mrs?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

理解函数式编程的逻辑要容易得多。这就像按顺序阅读句子一样。一个有用的类比就像一本烹饪书。命令式编程就像得到一步一步的指令去吃饭。而声明式编程(如函数式编程)就像获取食物的配料和描述。一步一步的指导比配料和膳食描述复杂得多。

**并行化**

![](img/c04c21f8fdf0b543439c8e68d66cf137.png)

Photo by [Taylor Vick](https://unsplash.com/@tvick?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

程序生成的状态是不可变的。因此每个处理单元将具有该状态的唯一原始副本。[你不需要使用任何声明性的方法来执行代码的并行化，比如线程锁定等。](https://www.pythontutorial.net/advanced-python/python-threading-lock/)使用正确的架构将代码并行化会很容易。

您可以通过将一系列作业放入 [GNU Parallel 来轻松地并行化您的代码。](https://www.gnu.org/software/parallel/)例如，假设您想要使用一个 For 循环来运行一组作业。有了这个程序，你可以同时运行每项工作，并重新组合输出，以节省时间。这种技术被超级计算机用来加速多个作业的完成。

[你可以阅读我的博文，了解管道等函数式编程特性如何使分布式计算的并行化变得更加容易。](/geekculture/distributed-computing-for-beginners-f4116adf609d)

## 你为什么不想用烟斗？

**更高的内存消耗**

![](img/121d4491bb98cc4778b43fac40d62962.png)

Photo by [Luan Gjokaj](https://unsplash.com/@luangjokaj?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

[国家的重复是一把双刃剑。对于每个动作，都会产生一个新的状态。这当然会增加分配的内存量。但是，对于现代语言来说，这应该不是问题，因为它们会通过垃圾收集器自动删除未使用的状态。](https://stackoverflow.com/questions/4522304/does-functional-programming-take-up-more-memory)

## 有用的管道示例

下面是一些在 Linux 系统上使用 shell 脚本中的管道的例子。

**快速运行一个目录下的可执行脚本**

有时你会有一堆需要运行的脚本保存在一个文件夹中。您可以运行以下命令来运行目录中的所有可执行脚本。

```
find "./" -name "*.sh" -type f -perm /a=x | xargs -L 1 bash
```

*   查找每个包含
*   运行这些文件

**快速查杀一组进程**

诸如脚本之类的进程可以产生一组进程。假设您有一个脚本，它从一个脚本运行大量备份，但您想要取消该脚本。您可以运行如下所示的命令。

```
ps aux | grep rsync | awk '{ print $2 }' | xargs -L 1 kill
```

*   过滤掉不包含 rsync 的进程
*   取出所有的处理 ID
*   参数化下一个命令的输出
*   取消所有进程

当然你可以运行类似

```
killall rsync 
```

但是，您不能选择性地终止任务，因为该命令会终止所有 rsync 任务。

IT 和工程领域是快速发展的领域。跟不上意味着你将被落在后面。跟上的最好方法是保持最新的新闻和教育内容。[订阅免费电子邮件列表，将您的职业生涯提升 10 倍。](/subscribe/@dretechtips)

**加入我们，因为 50 多名想要快速跟踪其职业生涯和知识基础的人已经注册。**

**相关内容**

*   [Unix 文件系统讲解](/geekculture/unix-file-system-explained-9d3554de9d46)
*   [如何在任何 Linux 操作系统上重置 root 密码？](/@drechang/how-to-reset-the-root-password-on-any-linux-os-7b2075eed7dc)
*   [Linux 内核初学者指南](/geekculture/the-beginners-guide-to-the-linux-kernel-29743b1a2daf)