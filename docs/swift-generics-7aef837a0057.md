# Swift |仿制药

> 原文：<https://medium.com/geekculture/swift-generics-7aef837a0057?source=collection_archive---------20----------------------->

## swift 仿制药指南

![](img/e018a43f423cc2c68cfb6fcfb2845e22.png)

Photo by [Kyle Glenn](https://unsplash.com/@kylejglenn?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/unknown?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

泛型是 Swift 中最强大的功能之一。它们允许您在不知道数据类型的情况下操作数据。使用泛型，您可以编写高度可重用和灵活的代码，实现简洁和抽象的表达式。信不信由你，在使用数组和字典等集合类型时，你已经在使用泛型了。

# 泛型示例

假设你有两个数组

```
var array1 :Array<Int> = []
var array2 :Array<String> = []
```

一个是 Int 类型，另一个是 String 类型。假设您想要添加到这些列表中

```
array1.append(7) 
 // [7]array2.append("pineapples") 
//["pineapples"]
```

array1 的 append 方法接受整数值，array2 的 append 方法接受字符串。这是否意味着 apple 为每种数据类型创建了许多 append 方法？不，他们用的是仿制药。

让我们更深入地了解一下苹果的数组 API

```
struct Array<E>{
   func append(_ element: E){ 
        .....
   }
}
```

# 无商标消费品

您可能已经知道，在使用数组时，必须在初始化数组之前确定数据类型。这就是数组是泛型类型的原因。如果您想要使用泛型，您可以通过在单箭头括号之间写入泛型的类型参数来指示您想要使用它们。

泛型可以用在任何需要指示数据的地方，比如类、结构和函数。

让我们创建一个接受泛型类型的简单函数

```
func printGeneric<E>(value: E){ 
     print("The generic value is \(value)"
} printGeneric(value: "Apples") --> "The generic value is Apples"
printGeneric(value: 100) --> "The generic value is 100"
```

# 通用和协议

您可以使用某些协议(如 Numeric 或 StringProtocol)来约束想要接受的值。

```
func printGeneric<E:StringProtocol>(value: E){ 
     print("The generic value is \(value)"
}printGeneric(value: "Apples") --> "The generic value is Apples"
printGeneric(value: "a") --> "The generic value is a" 
```

一个函数接收字符串，另一个接收字符。如果将参数设置为 Int 或 Double 会怎么样？你会以一个错误结束。

这就是泛型的全部！感谢您的阅读！！！如果你有任何问题，请发邮件到 yu24c@mtholyoke.edu 给我