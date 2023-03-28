# Swift | Swift 中的高级协议

> 原文：<https://medium.com/geekculture/swift-advanced-protocols-in-swift-823b935a6382?source=collection_archive---------2----------------------->

## 使协议符合扩展、协议继承、仅类协议以及向协议添加扩展

![](img/ed3dd6ce2cd710022acac284d951d092.png)

Photo by [Sven Mieke](https://unsplash.com/@sxoxm?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/blueprint?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 使协议符合扩展。

扩展可以符合协议。

例如，字符串是结构。通过使字符串符合您自己的协议，您可以扩展字符串的功能。在这个例子中，我使用 name 属性作为符合 Person 协议的类型。然后，我使用 extension 关键字在字符串结构中实现了 person 协议的功能。

# 协议继承

一个协议可以继承多个协议，并添加比现有协议更多的要求。

协议超级计算器符合另一个称为计算器的协议。因此，因为 myClaculator()类符合 superCalculator，而 super Calculator 符合 calculator()，所以我们需要实现这两种协议的属性和方法。

协议也可以继承多个协议，如下例所示。

ReadWriteSpeakable 协议符合 read and write 协议，因此当我们实现符合 ReadWriteSpeakable 协议的类/结构/或枚举时，我们需要实现所有三个协议中的所有方法。

# 仅限类别的协议

通过将 class 关键字添加到协议的继承列表中，您可以将协议限制为仅用于类类型，要限制为仅用于类的协议，请将 class 关键字放在协议的继承列表的开头。

正如你所看到的，fullCalculator 继承了 class，这是一个 classOnly 协议。这意味着只有类才能符合协议。

# 向协议添加扩展

您不能实现协议的功能，但是您可以通过扩展它们来实现。当您扩展一个协议时，当您添加功能时，您不必在您的结构/类/或枚举中重新定义它们。

在上面的例子中，我们有一个叫做 add 的协议。结构 addingCalc()符合 add 协议，因此它必须实现其功能。函数 printAdd()是 Add 协议的扩展，因为它是一个扩展，所以可以实现。这意味着我们不必在结构中实现 printAdd()，我们可以马上调用它。