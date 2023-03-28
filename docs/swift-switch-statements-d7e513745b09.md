# Swift | Switch 语句

> 原文：<https://medium.com/geekculture/swift-switch-statements-d7e513745b09?source=collection_archive---------14----------------------->

## Break、Fallthrough、where、enums 等

![](img/7efd73d87d4b5292745ef4bd1a9e15ce.png)

Photo by [Isabella and Zsa Fischer](https://unsplash.com/@twinsfisch?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/switch?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如果你开始在 SwIft 中开发，你会注意到你比 if，then 语句更多地使用 Switch。Switch 语句更容易阅读，也更适合用来比较值。今天，我将深入探讨 Switch 的基础知识。

使用 switch 语句的基本格式如下所示

```
**switch** inputValue{
    **case** comparisonValue: 
         /*code*/
    **default**: 
         /*code*/
}
```

# 转换

当使用 else 和 else if 有多个条件时，应该考虑使用开关情况。使用这种方法，您可以编写一次条件，列出所有可能的情况及其结果。每种情况下的比较值必须与输入值具有相同的数据类型。您应该调用 default，除非这些值是明显受限的值，如 enums(稍后讨论)。

让我们比较 if、else 语句和 switch 语句，观察它们的区别。

## 突破和失败

如果您熟悉 C 或 Java 中的 switch 语句，您应该熟悉这样一个事实，即必须为每种情况编写 break 语句来退出 switch 语句。

这是一个用 Java 编写的示例代码

在 Swift 中，添加 break 语句是可选的，因为它是在每种情况下自动设置的，所以当情况为真时，它会自动退出语句。但是，如果希望忽略中断值并继续执行下一个代码，可以将 fallthrough 添加到语句中。在 switch 语句中可以多次使用 Fallthrough。

## 元组

在用通配符标识符(_)比较值时，元组非常有用。

## where 关键字

当您想要扩展案例的条件时，使用 Where 关键字。where 的语法将在另一篇文章中介绍。但是现在，我将使用下面的例子。switch 语句只比较食物，但是我们可以比较更多的值，比如 price 和 isDelicious，这就是 where 关键字有用的地方。

## 将开关与枚举一起使用

当接收范围有限的值(如枚举)时，如果实现每个相应的值，则不必添加默认大小写

## 默认替换

您可以用以下内容替换默认标识符

1.  案例 _:

```
case _: 
   print("Replacement 1") 
```

2.@未知案例 _:

```
@unknown case _: 
   print("replacement 2") 
```

这两个也表现得像违约。

# 总结:

*   Switch 是一种三元运算，它根据真或假来执行值
*   除非比较非常具体的值(如枚举)，否则必须编写 default 语句
*   它会自动对每个案例进行分段，而不会给案例添加分段。
*   您可以使用 fallthrough 关键字来覆盖中断。
*   您可以根据需要多次使用 fallthrough。
*   你想写多少案例都可以

谢谢你看我的帖子！如果你有任何问题，请发电子邮件到 yu24c@mtholyoke.edu 给我！