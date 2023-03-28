# 南 O.L.I.D 原则 3:利斯科夫替代原则。

> 原文：<https://medium.com/geekculture/s-o-l-i-d-principle-3-liskov-substitution-principle-28250ad38bbe?source=collection_archive---------21----------------------->

![](img/9a7c69c29e0749538b9d0ac48c50b594.png)

Photo by [Fotis Fotopoulos](https://unsplash.com/@ffstop?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Barbara Liskov 和 Jeannette Wing 在 1994 年定义了最初的原则:

> 子类型要求:设φ(x)是关于 t 类型的对象 x 的一个可证明的性质。那么φ(y)对于 S 类型的对象 y 应该是真的，其中 S 是 t 的子类型。

简单地说，任何子类都应该能够用它的所有方法和属性完全替换它的父类。例如，让我们看一个表示矩形的类。它有设置其高度和宽度的方法:

现在让我们创建一个新的类，它从一个矩形扩展而来，名为:Square:

我们看到可重写方法`setWidth`的操作方式与其父类中的方法不同，因为它也改变了 height 属性。这明显违反了 Liskov 替换原则，因为我们不能简单地用它的子类替换父类并期望得到相同的结果(在我们的例子中，得到 Height 属性值)。

有两种可能的方法来克服这个问题:

创建一个独立的 Square 类，它不是从 rectangle 类派生的。

创建一个四边形(有四条边的多边形)基类。该类将具有以下属性:

1.  边 1，边 2，边 3，边 4
2.  我们将编写计算四边形面积和周长的函数。虽然周长计算很简单(所有四条边的总和),但面积计算可能相当复杂，更多详细信息请访问:[https://byjus.com/maths/area-of-quadrilateral/](http://b.	We will write the functions of calculating the area and perimeter of the Quadrilateral. While the perimeter calculation is simple (sum of all four edges) the area calculation can be quite complex. ,more details can be found at: https://byjus.com/maths/area-of-quadrilateral/)
3.  现在我们可以创建从四边形基类派生的子类(例如梯形、矩形、正方形等等)。面积法和周长法的工作方式完全相同，得到的结果也完全相同。
4.  当然，我们可以在每个子类上添加专用的方法，但归根结底，我们将能够用基类替换任何子类，并期望得到相同的结果。