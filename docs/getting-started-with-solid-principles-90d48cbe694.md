# 从坚实的原则开始

> 原文：<https://medium.com/geekculture/getting-started-with-solid-principles-90d48cbe694?source=collection_archive---------20----------------------->

![](img/0eecf55b8db815892ac72ade442b6ee2.png)

Photo by [Christina @ wocintechchat.com](https://unsplash.com/@wocintechchat?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

今天的软件应用程序极其复杂。在许多情况下，一组开发人员开发应用程序。自然，可能会出现某些问题:

1.  新团队成员学习应用的难点:现实生活中，团队成员来来去去。在包含复杂模块、类和接口的复杂应用程序中。自然，新团队成员学习应用程序会很困难。
2.  在大型项目中，经常有必要修改经常使用的类或模块，并且许多其他对象可能依赖于它。如果我们修改了这个类，那么我们将把应用程序暴露给可能由此产生的新的潜在错误。
3.  如果我们修改一个许多其他类依赖的类，那么将会有破坏其向后兼容性的风险。依赖于此类的其他类可能会出现意外行为。
4.  如果我们要构建许多其他对象都依赖的对象，那么父组件就有可能不需要或者不能使用它们的所有功能。代码会很繁琐，充满了不必要的函数。
5.  还有一个风险是，如果我们改变一个特定对象中的某个东西，那么也有必要改变它的依赖对象。

在 2000 年，Robert C. Martin(也被称为 Bob 叔叔)发表了一个文档，其中包含了设计软件项目的五个原则。这里是到原始文档的链接。这五项原则后来被称为 S.O.L.I.D 原则。以下是这五个原则的列表。有关每项原则的更多详情，请点击每项原则的链接:

1.  [单一责任原则](https://krasnoff-kobi.medium.com/s-o-l-i-d-principle-1-single-responsibility-principle-83af735cc62e):每个类、模块或对象都应该对一个任务负责。
2.  [开闭原则](https://krasnoff-kobi.medium.com/s-o-l-i-d-principle-2-open-closed-principle-3658231a1a09):一个类、模块或对象可以开放扩展，但关闭修改
3.  [Liskov 替换原则](https://krasnoff-kobi.medium.com/s-o-l-i-d-principle-3-liskov-substitution-principle-28250ad38bbe):任何子类都应该能够用它的所有方法和属性完全替换它的父类。
4.  [接口分离原则](https://krasnoff-kobi.medium.com/s-o-l-i-d-principle-4-interface-segregation-principle-9c2325cfc310):组件不应该依赖于它不使用的属性和方法。
5.  [依赖倒置原则](https://krasnoff-kobi.medium.com/s-o-l-i-d-principle-5-dependency-inversion-principle-238276db379f):高层次模块不应该依赖低层次模块。