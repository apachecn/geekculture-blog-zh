# 南 O.L.I.D 原则 4:界面分离原则。

> 原文：<https://medium.com/geekculture/s-o-l-i-d-principle-4-interface-segregation-principle-9c2325cfc310?source=collection_archive---------18----------------------->

![](img/1e04c273d7e7d4fb9b48eccf11ab2409.png)

Photo by [Hack Capital](https://unsplash.com/@hackcapital?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

任何组件都不应该依赖于它不使用的属性和方法。让我们通过一个代码示例来了解这一点:

我们看到一个有五个方法的接口。现在让我们构建一个表示电动汽车的类，它实现了初始接口:

当我们在一个类中实现一个接口时，我们也必须实现它的所有方法。我们实现的方法之一是`refuel` 方法。因为我们不能给电动汽车加油，这种方法有一个问题，因为我们无论如何都要实施它。

这个问题的解决方法很简单。代替一个基本接口，我们将创建三个接口:一个接口将拥有所有汽车类型的所有公共方法，一个接口实现`refuel` 方法，一个接口实现电动汽车的充电方法:

我们新的 ElectricCar 类将只实现`ICar` 和 IBatteryCharge 接口，因此不必实现`refuel` 方法: