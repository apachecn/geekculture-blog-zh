# Swift | Inout 参数和&符号。

> 原文：<https://medium.com/geekculture/swift-inout-parameters-and-ampersand-a515e3f0ac50?source=collection_archive---------17----------------------->

## inout 参数和&符号指南

![](img/afd4acadf5416013633b515a24f5af1f.png)

Photo by [Mark Wieder](https://unsplash.com/@poprietor?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/ampersand?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

functions 参数本质上是一个常量，试图在函数体中更改该参数的值会导致编译错误。如果希望修改函数中参数的值，并在函数调用完成后保留更改，可以使用 inout 参数。

**inout** 参数将值传递给函数，由函数修改，然后传递回 it 以替换原始值。

Inout 参数只能传递给变量 var，常量和文字不能作为参数传递，因为它们不能被修改。在变量名前使用一个&符号表示可以修改。

下面是一个例子:一个切换 A 和 b 的值的函数

```
**var** numberOne: Int = 1**var** numberTwo: Int = 2 swapTwoInts(&numberOne,&numberTwo)print(numberOne) //2print(numberTwo) //1
```

本节到此为止！感谢您的阅读！如果你在 yu24c@mtholyoke.edu 有任何问题，请告诉我。