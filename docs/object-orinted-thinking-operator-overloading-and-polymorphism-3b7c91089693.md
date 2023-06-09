# 面向对象的思维:运算符重载和多态

> 原文：<https://medium.com/geekculture/object-orinted-thinking-operator-overloading-and-polymorphism-3b7c91089693?source=collection_archive---------25----------------------->

即使是经营者也需要承担额外的责任

![](img/da07f6ef5c9498f06d974dafd1175572.png)

Overloaded ([https://www.123rf.com/](https://www.123rf.com/))

首先，欢迎来到面向非技术专业人士的 Python 编程入门系列的最后一部分。我很高兴写这篇文章，我猜你也很高兴看到这篇文章。我们已经走了很长的路。最棒的是，我们可以用一个相对简单和常见的概念来结束这个系列。

这段代码中提供的所有代码片段和输出都存储在我的 [Github 存储库](https://github.com/arvindhhp/PyPro_ahhp/blob/main/Part_017_Operator_Overloading_Polymorphism.ipynb)中。

## 抛开那些花哨的术语，让我们先试着思考一下超载意味着什么？

当某个实体被要求执行比预期更多的任务时，它就会过载。该实体已被分配执行一些预定义的操作，但由于某些附加要求，一些附加操作落到了它的头上。就像一个像你这样有责任心和责任感的专业人士一样，当新的职责出现时，你有时会被工作压得喘不过气。

嗯，同样的事情也可能发生在运营商身上。根据数学，加号(加法运算符)，“+”应该将两个数字相加。但是，如果我们在一个类中定义了一个特定的方法实现，强制加号将两个输入相乘。听起来很奇怪，对吧？？但在这里，我只是试图给一个什么是运营商超载的感觉。我们很快就会看到实际情况。

## 那么多态性呢？

另一个非常常见的术语，指的是某个实体采取各种形式的能力的艺术。好吧，让我把自己看作一个实体，我在工作时是一个流体系统分析师，在周末是一个懒惰的人，在写这篇文章时是一个试图交流 Python 的学生等等。，懂了吗？？我们自己每天都在展示多态性的概念。

类似地，在编程中，我们可以让相同的对象在不同的实例中执行不同的操作。例如，在一个类中定义的同一个方法可以为不同的参数产生不同的返回值，这听起来是显而易见的，但这只是显示的多态性。再比如，让我们回到加号，加法运算符(+)，就像我们上面讨论的，忽略运算符重载部分，只考虑我们都知道的加号。它可以对数字输入执行算术加法，对字符串输入执行字符串连接，对列表输入执行列表追加，很有趣，不是吗？？

没错，这就是 Python 实现多态性的方式。

是的，我能猜到你的想法，我们可以简单地认为，操作符重载只是多态的一种形式，我们指定操作符应该做什么。我们很快会详细讨论这一点。

那么让我们跳到操作符重载。

# 运算符重载

## 重要的事情先来

为了利用操作符重载的特性，我们需要使用特定操作符的内置方法实现。我们只是在这里执行一个方法覆盖。正如我们所知道的，所有用户定义的类，所有在它里面定义的方法，无论是什么，默认情况下都成为' main '类的一部分。

简单来说，所有用户定义的类都成为主类“main”的子类。

让我展示一个同样的例子。

Understanding __main__(), for Output, refer [here](https://github.com/arvindhhp/PyPro_ahhp/blob/main/Part_017_Operator_Overloading_Polymorphism.ipynb)

你注意到输出了吗,“哑”类是如何表示的。它被表示为内置类 main 的子类。

那么，我们为什么要讨论这个操作符重载呢？？

所有操作符的默认功能都在这个“main”类中定义。当我们使用一个用户定义的子类中的方法重新定义任何操作符的实现时，我们只是在这个方法中局部地忽略了它的实现。

不过理解实现要稍微复杂一些。通过在两个类对象之间执行所需的操作，可以利用更新的运算符实现。

**让我们现在为加法运算符(+)** 实现它

Addition Operator Overloading Definition

> 我完全接受，你们中的许多人现在可能会迷失，看，很快就会明白的。

如果没有重新实现 **add** 方法，Object1 和 Object2 之间的一个简单的(+)将导致一个类型错误。这绝对可以理解。我们正在尝试一些用户定义的类的两个实例。加法运算在这里没有作用。在执行这样的加法时，解释器会告诉我们，我们正在使用+操作符来处理不支持的操作数。

但是在这里，我们超越了预定义的定义。我们现在告诉大家，如果添加了这个类的两个对象，请实现这个类中的 **add** ()返回的任何返回语句。

永远记住，这只能对修改了内置函数的同一个类的两个对象执行。此操作中涉及的对象数量取决于运算，如加、除、乘等算术运算。是二元运算(有两个操作数)，因此有两个对象。同样的想法可以扩展到比较、辅助甚至一元运算符。

Addition Operator Overloading Implementation

各种运算符的可用内置实现列表的快速参考

[](https://www.geeksforgeeks.org/operator-overloading-in-python/) [## Python - GeeksforGeeks 中的运算符重载

### Python 操作符重载中的操作符重载意味着赋予它们预定义操作符之外的扩展含义…

www.geeksforgeeks.org](https://www.geeksforgeeks.org/operator-overloading-in-python/) 

# 多态性

嗯，这里没什么好讨论的了。但是让我只展示一个简单的方法实现，它接受两个参数并返回加法运算的结果。

Polymorphism Implementation

# 恭喜

我们已经走了这么远。

如果您一直在学习面向非技术工程师的 Python 入门系列，我想说，我们已经掌握了 Python 标准 I/O 的所有基础知识，包括各种数据类型、循环、函数、文件处理或目录处理、日志记录、异常处理，最后还有 oops 概念的基础知识。

完整回购@[https://github.com/arvindhhp/PyPro_ahhp](https://github.com/arvindhhp/PyPro_ahhp)

我提出这一序列背后的基本想法是，让核心工程师(如机械、民用和其他专业，无论其领域如何)能够执行基本编程，以便可以通过机器学习、复杂数据可视化来开发数据的力量，从而增强现有能力。

理解了本系列中涉及的 Python 编程概念后，我们可以认为自己有资格继续学习利用数据的艺术。