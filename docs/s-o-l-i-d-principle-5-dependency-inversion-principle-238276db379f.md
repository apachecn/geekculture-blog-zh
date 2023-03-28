# 南原则 5:依赖性倒置原则

> 原文：<https://medium.com/geekculture/s-o-l-i-d-principle-5-dependency-inversion-principle-238276db379f?source=collection_archive---------22----------------------->

![](img/4c0c2f7d508e5d7fba908b353d25188b.png)

Photo by [Lukas Blazek](https://unsplash.com/@goumbik?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

高级模块不应依赖低级模块:

在上面的代码示例中，我们有两个类:`AppComponent`，它是高级模块，依赖于`MySqlDBClass`。这个应用程序可能看起来工作正常，而且在很多情况下也确实如此。然而，低级类与高级类紧密耦合。这种情况会导致问题。

如果有新的需求出现。例如，当我们必须用 MongoDB 服务器替换 MySQL 服务器时。有一个风险是，我们将不得不更改高级组件(在构造函数方法中，我们将不得不更改 DB 类定义),从而增加编码时间。

解决方案是使两个模块的连接成为松耦合连接:

这里我们可以看到`AppCompoent` 不是直接连接到`MySqlDBClass` 的，而是通过 IDBClass 接口连接的。在这种情况下，如果需要更改数据库并用 MongoDBClass 类替换 MySqlDBClass，我们不需要更改`AppComponent` 中的任何内容。

也可以对整个模块执行单元测试。我们可以在一个场景中编写两个测试，而不需要修改`AppCompoent` 类。