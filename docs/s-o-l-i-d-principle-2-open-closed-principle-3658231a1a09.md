# 南 O.L.I.D 原则 2:开闭原则。

> 原文：<https://medium.com/geekculture/s-o-l-i-d-principle-2-open-closed-principle-3658231a1a09?source=collection_archive---------13----------------------->

![](img/537653c246a1b1ce7cb3394427c873d0.png)

Photo by [Karl Pawlowicz](https://unsplash.com/@karlp?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

一个类、模块或对象可以对扩展开放，但对修改关闭。为了理解这句话，我们必须理解几个术语:

1.  “为修改而关闭”:这意味着已经被调试、测试和被其他类和模块使用的类、模块或对象不应该被改变。我们不允许通过添加新的属性、方法、函数等等来修改它。
2.  “Open for extension”:有几种方法可以修改现有的类。其中之一就是写一个新的类，这个类继承自原来的类。通过这种方式，我们不会改变我们最初测试良好的类。

我们现在将构建一个模块，它连接到一个数据库并可以在其中执行各种查询。在第一阶段，我们将只建立对一个数据库(MySQL 数据库)的访问。

在第一阶段，我们将构建一个抽象类，它为所有连接到数据库的类定义一个模板。这个抽象类将包含三个方法:

1.  连接
2.  拆开
3.  执行查询。

现在我们可以构建一个类，它继承了我们的抽象类:

现在我们将构建主类(AppComponent)，它设置一个实例并从`MySqlDBClass` 类获取数据:

过了一段时间，我们可能会有新的需求:我们也需要访问 MongoDB 数据库。

因为我们遵守开闭原则，所以我们不能修改`MySqlDBClass` 类(修改时关闭),但是我们可以构建一个从基类扩展的新类(扩展时打开)。新班级的名字是`MongoDBClass`。

我们将向主类添加一个新的`MongoDBClass` 实例，并执行程序: