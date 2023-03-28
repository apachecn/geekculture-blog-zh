# Swift|内联闭包

> 原文：<https://medium.com/geekculture/swift-inline-closures-eb1513f304a?source=collection_archive---------7----------------------->

## 内联闭包完全指南

![](img/e5e4451f5c1e4061c67aad1764e86a27.png)

Photo by [NeONBRAND](https://unsplash.com/@neonbrand?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/%24?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Swift 自动为内联闭包提供短参数名。

此参数允许您使用$0、$1、$2 等值。如果使用缩写的参数名称，可以省略参数输入部分，因为您知道参数值和处理参数时使用的参数是相同的。

在开始之前，让我们看看用闭包调用函数的不同方式。

有三种方法可以调用下面的函数

```
func morningRoutine(one: String, two:(String)->String){
       print(one) 
       let secondTask = two("Brush Teeth"
       print(secondTask) 
       print("Finished") 
}
```

# 第一选项

```
morningRoutine(one:"Wake up and wash face") { (toDo: String) -> String in 
    return "second task is \(toDo)"
}
```

这是最简单易懂的方式。

# 第二种选择

```
morningRoutine(one:"Wake up and wash face") { toDo -> String in 
    return "second task is \(toDo)"
}
```

Swift 可以删除闭包的参数，因为它知道闭包的参数必须是字符串

# 第三种选择

```
morningRoutine(one:"Wake up and wash face") { toDo in 
    return "second task is \(toDo)"
}
```

Swift 可以删除闭包的参数类型和返回值，因为它知道它接受并返回一个字符串。

# 第四种选择

```
morningRoutine(one:"Wake up and wash face") { toDo in 
    "second task is \(toDo)"
}
```

您可以去掉 return 关键字，因为闭包只有一行代码返回值。但是如果你有其他代码，不要使用这个方法，你会遇到一个错误。

# 第五种选择

```
morningRoutine(one:"Wake up and wash face") {
    "second task is \($0)"
}
```

您可以使用速记语法使其更短。您可以让 Swift 为您的闭包自动命名，而不是编写 toDo。他们有一个美元符号，后面跟着一个从 0 开始的数字。

# 何时使用速记参数

当使用闭包时，Swift 提供了一个特殊的短参数语法，它考虑到了编写闭包。该语法自动对参数名$0、$1、$2 等进行编号。

例子

```
Calculator(n1: Int, n2: Int, operation(Int, Int) -> Int{ 
     print(operation(n1,n2) 
}
```

这个函数有一个接受两个参数的闭包，所以我们可以用$0 表示第一个参数，用$1 表示第二个参数

```
Calculator(n1:100, n2: 2) {
    $0 * $1
}//OUTPUT
// 200
```