# Swift |协议

> 原文：<https://medium.com/geekculture/swift-protocols-6419b71b7d13?source=collection_archive---------9----------------------->

## 协议中的属性、方法和初始值设定项要求

![](img/c61d1bc6b6ab6825fa9e56c8ab0f1f6e.png)

Photo by [Kaleidico](https://unsplash.com/@kaleidico?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/blueprint?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Swift 被认为是面向协议的编程。为了理解面向协议编程的概念，我们需要理解什么是协议。

# **协议**

协议是特定任务或功能的方法和属性的蓝图。它们只定义和呈现，本身不实现功能。结构、类和枚举符合协议，并负责实现它们符合协议的特定功能和任务。一个类、结构或枚举可以符合多个协议。

```
protocol FirstProtocol{......}
protocol SecondProtocol{....}struct example1: FirstProtocol, SecondProtocol{ ......}
```

如果使用类并继承。您必须首先指出超类，然后是协议。

```
class example2: Superclass, FirstProtol, SecondProtocol{......}
```

# 协议中的属性要求

```
protocol someProtocol{
     var gettableAndSettable: String{get set} 
     var gettableOnly: Int{get} 
     static var typeProperty: Double{ get}}
```

协议中的属性有实例属性和类型属性，如**符合类型**。当在协议中调用属性时，它应该指定类型(是实例还是静态属性)、名称，以及它是否只是一个 getter，还是 getter 和 setter。协议要求将它们的属性设置为变量。

**举例**

如果协议中的属性定义为 get only，则它可以实现为计算属性或存储属性。但是当它被实现为 get 和 set 属性时，它应该被实现为 computed 属性。

# 方案中的方法要求

```
protocol someProtocol{
      func instanceMethod() 
      func instanceMethod1(name:String, age: Int) 
      func instanceMethod2(name:String) -> String 
      static func typeMethod1()}
```

就像属性一样，协议有实例和类型方法作为**符合类型**。方法与常规的实例和类型方法相同，但是没有主体和花括号。定义这些方法时，不能指定参数默认值，但参数值的编写方式与常规方法相同

## 类中的类型方法

类型方法在类中实现时会更复杂，因为类方法可以被重写。一个类可以使用 class 关键字来允许子类覆盖超类对类型方法的实现。因此，当您希望符合将类型方法作为符合类型的协议时，可以使用 static 或 class 关键字来实现这些方法。

*   **静态**关键字→无法覆盖子类。
*   **class** 关键字→能够覆盖子类。

上面的代码显示了如何在类中使用类型方法作为一致性类型的协议。someClass()是父类，anotherClass1()是子类。someClass()符合 somProtocol()协议，该协议有两个类型方法。我选择用**静态**关键字实现 someTypeMethod()，用**类**关键字实现 anotherTypeMethod1()。由于 anotherTypeMethod1()是带有 class 关键字的类型方法，因此子类可以重写它。

## **变异方法要求**

当通过实例方法(在枚举和结构中)更改值类型的内部值时，请在 methods func 关键字前使用 mutating 关键字，以指示该方法更改了内部值。

如上所述，协议有一个变异方法作为一致性类型。当协议符合一个结构时，您需要使用 mutating 关键字来实现它。然而，当它符合一个类时，您不需要这样做。

# 协议中的初始化器要求

协议可以有初始化器。要请求一个初始化器，就像方法请求一样，你需要定义一个初始化器并实现它。换句话说，您只需要指定参数。

当初始化器在协议中作为一致性类型被调用时，它们在结构中被调用时不需要**必需的**关键字。但是，required 关键字必须在类中使用。

实现协议要求以执行特定功能。

*   协议定义了适用于特定任务或功能的方法、属性和其他要求的蓝图。
*   结构、类和枚举符合协议，并且实际上可以实现协议要求来执行特定的功能。
*   协议只是定义和呈现，本身并不实现功能。只定义了条件。
*   多个协议可以符合一个结构、类或枚举。
*   协议只定义方法、属性和初始化器。只要符合这些标准，它们就会在任何地方得到实施。