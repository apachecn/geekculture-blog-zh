# 软件设计模式#6:单一模式

> 原文：<https://medium.com/geekculture/software-design-pattern-6-singleton-580f50415bdc?source=collection_archive---------17----------------------->

设计模式是可重用的模板，帮助我们使用最佳实践解决软件设计问题。通过这种方式，它们帮助我们使用更易于维护、理解、重用和测试的代码来构建应用程序。

# 逃离速度实验室

你可以在我们的网站上找到我们所有的文章、课程和教程:
[https://www . ev labs . io](https://www.evlabs.io/)

![](img/70ea5d4ff5179d20a564e435ccba7265.png)

# 这个图案是干什么用的？

> 这种模式将一个类的对象的创建限制在一个实例中。

继续我们的社交网络应用程序的例子，在这篇文章中，我们将实现一个类，负责在外部文件中记录应用程序的操作，这样当用户有错误时，我们可以调试它。

这个名为 *Logger* 的类将记录我们应用程序中发生的所有事情，所以它必须可以从所有的类中访问。这给我们带来了一个问题。为了在应用程序的每个类中创建 Logger 类的实例，我们必须将 *Logger* 的依赖项作为参数传递给所有其他类。

```
**class SQLiteDatabase**: **def** **__init__**(self, dep_a, dep_b):
        self.logger = Logger(dep_a, dep_b)
        ...
```

正如您在上面的类中所看到的，我们必须将日志依赖项传递给数据库，尽管它们与数据库本身没有任何关系。另一种可能性是将记录器实例直接注入数据库:

```
**class SQLiteDatabase**: **def** **__init__**(self, logger):
        self.logger = logger
        ...
```

但是，正如我们所说的，Logger 将出现在应用程序的几乎每个类中，所以我们的代码仍然是不必要的复杂。

解决方案:创建 Logger 类的单个实例，并从将要使用它的类内部调用它。

# 它是如何工作的？

第一次调用 Logger 类时，我们将创建这个类的唯一实例。后续调用将简单地返回该实例:

```
**class Logger**:
    __instance = **None**     **def** __new__(cls, *args):
        if cls.__instance is **None**:
            cls.__instance = object.__new__(cls)
        **return** cls.__instance
```

如您所见，Logger 类将存储一个名为 *__instance* 的隐藏变量，一旦实例被创建，我们将在其中存储该实例。 *__new__* 方法负责检查该实例是否存在，如果不存在，它将创建它。

在我们使用 Logger 的所有类中，我们将简单地获得唯一的实例并正常使用它，而不必将该类暴露给 *Logger 的*依赖项:

```
**class Database**:    

    **def** __init__(self):
        self.logger = Logger() **def** insert(self, row):
        self.logger.log('Starting insertion of a row.')
```

# 利益

*   我们确保只有一个可能包含敏感资源的类实例。
*   允许从应用程序中的任何位置访问。
*   延迟对象的创建，直到需要使用时(见[模式#4:延迟初始化](/geekculture/software-design-pattern-4-lazy-initialization-35f606f1ddf3))。

# 不足之处

*   这使得对应用程序不同部分的单元测试变得困难，因为需要创建模拟对象来替换所使用的单例。
*   我们将应用程序的所有类绑定到 Logger 类的使用。如果以后我们引入另一个名为 *Logger2* 的类，我们将不得不替换应用程序中使用 Logger 的所有点。

一般来说，这种模式由于其缺点而备受争议。因此，不鼓励使用它，除非它的状态不影响应用程序的功能(如日志工具)。