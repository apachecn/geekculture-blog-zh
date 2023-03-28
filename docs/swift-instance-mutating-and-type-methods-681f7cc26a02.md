# Swift |实例、变异和类型方法

> 原文：<https://medium.com/geekculture/swift-instance-mutating-and-type-methods-681f7cc26a02?source=collection_archive---------15----------------------->

![](img/34ffdf1d3064811c76a58323066b5f53.png)

Photo by [Nathalia Segato](https://unsplash.com/@nathsegato?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/methods?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在类、结构或枚举内部编写的函数称为方法。结构、类和枚举可以为自己定义封装特定操作或功能的方法。

```
func outsideFunction(){ 
 ....}class someClass{
   func insideFunction(){ 
     .... } }
```

outsideFunction 是一个函数，insideFunction 是一个方法。

# 实例方法

实例方法是属于特定类、结构或枚举的实例的函数。Ir 提供了访问和修改与实例相关的属性或功能的方法。

*   实例方法只能在实例存在时使用。如果没有现有的实例，就不能调用它
*   实例方法隐式访问该类型的所有其他实例方法和属性
*   实例方法只能在它所属的特定实例类型上调用

定义了一个名为 counter 的类。它定义了 3 个实例方法 increment()、increment(By: Int)和 reset()

```
let counter = Counter()// count = 0 

counter.increment()//count = 1

counter.increment(by: 5)//count = 6

counter.reset()//count = 0
```

# 突变方法

当创建结构或枚举的实例时，其属性不能更改。当您创建一个 struct 时，Swift 不知道是将它与常量还是变量一起使用，因此您应该在默认情况下采取一种安全的方法。

如果你想修改一个结构的内部属性，你必须在实例方法中使用**突变**关键字来修改它。

```
var animal = Animal(name: "Max") //"Max"
animal.makeAnonymous() //"No Name" 
animal.printName() 
```

# 类型方法

类型方法使用 static 关键字。这是在类型本身上调用的方法。静态函数不能被子类覆盖。类型方法指向它们自己的类型，所以它们不能改变类型内部的任何变量，除非它们被初始化为静态的。

*   实例方法指向它们自己的实例
*   类型方法指向它们自己的类型

```
**var** UniversityA = University(name: "A", founded: 1920)**var** UniversityB = University(name: "B", founded: 2000)University.leave()University.people
```

当使用 UniversityA 或 UniversityB 时，只调用 instanceMethods。当您想要更改关于大学类本身的信息时，请使用 Type 方法。

这就是我所有的方法。感谢你阅读这篇文章