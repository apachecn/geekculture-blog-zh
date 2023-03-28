# Swift 快速指南

> 原文：<https://medium.com/geekculture/quick-guide-to-swift-c6590e5407c6?source=collection_archive---------15----------------------->

iOS 编程语言

![](img/0f8a027b7f6c1ad24b35a9f293417f98.png)

Swift for iOS

# Swift 概述

Swift 是一种通用、多范例、面向对象、函数式、命令式和块结构语言。

Swift 的一些附加功能包括:

*   用函数指针统一闭包
*   元组和多个返回值
*   无商标消费品
*   支持方法、扩展和协议的结构
*   函数式编程模式，例如映射和过滤
*   强大的内置错误处理
*   带有`do`、`guard`、`defer`和`repeat`关键字的高级控制流程

Swift 使用自动引用计数(ARC)来管理内存。

## 类型安全和类型推理

Swift 是一种类型安全的语言，这意味着你不能将一个整数值赋给一个字符串变量，这将在编译时被检测到。

如果您没有使用类型注释，即使用变量声明数据类型，Swift 将从分配的值推断数据类型。

## 数据类型

Swift 有以下数据类型: **Int** ， **Float** 和 **Double** 用于存储数字。**字符串**，**字符**，**布尔**为布尔值，**可选**为存储值或零，**元组**为存储复合值。

## 声明变量

在 Swift 中，变量使用 **var** 关键字声明，常量使用 **let** 关键字声明。

```
// type of the variable can be inferred from the value
// a variable can be re-assigned
var name = "John"// Type annotation: declaring the variable with data type
var name: String = "John" // this is a constant and it's value can't be changed
let name = "Alan"
```

## 命名规格

常量和变量名不能包含空白字符、数学符号和箭头。它们也不能以数字开头，尽管数字可以包含在名称的其他地方。

## 字符串插值

这是一个奇特的名字，实际上是一件非常简单的事情:在一个字符串中组合变量和常量。

```
**var** name = "John Doe"
**var** age = 30
print("Your name is \(name) and age is \(age)")// Strings can be concatenated using '+' symbol
var firstName = "John"
var lastName = "Doe"
var fullName = firstName + lastName
```

# 选项

**Optionals** type 处理没有值的情况。期权要么说“有一个值，它等于 x”，要么说“根本没有值”。

```
var a: Int? = 4
var b: Int? = 5// This is an invalid operation because a and b can be nil
var x = a + b// Correct way to do this Optional binding (if let)
if let valueA = a {
   if let valueB = b {
      val result = valueA + valueB 
      print(result)
   }
}
```

## 可选链接

为了安全地访问包装实例的属性和方法，使用后缀可选链接操作符(后缀`?`)

```
if imagePaths["star"]?.hasSuffix(".png") == true {    
   print("The star image is in PNG format")
}
// Prints "The star image is in PNG format"
```

## 使用零合并运算符

在`Optional`实例为`nil`的情况下，使用零合并运算符(`??`)提供默认值。

```
let defaultImagePath = "/images/default.png"
let heartPath = imagePaths["heart"] ?? defaultImagePath
print(heartPath)// Prints "/images/default.png"
```

## 无条件展开

当您确定`Optional`的一个实例包含一个值时，您可以使用强制展开操作符(后缀`!`)无条件地展开该值。

```
let number = Int("42")!
print(number)
// Prints "42"
```

# 条件式

```
if x%2 == 0 {
   print("x is even")
} else  {
   print("x is odd")
}// **Ternary Conditional Operator**
x%2 == 0 ? print("even") : print("odd")// **Switch case** var secondaryColor = "green"
switch secondaryColor {
  case "green":
    print("Mix of blue and yellow")
  case "purple":
    print("Mix of red and blue") 
  default: 
    print("This might not be a secondary color.")
}// **Switch statement: Interval matching**
let year = 1905
var artPeriod: String

switch year {
  case 1860...1885:
    artPeriod = "Impressionism"
  case 1886...1910:
    artPeriod = "Post Impressionism"
  case 1912...1935: 
    artPeriod = "Expressionism"
  default:  
    artPeriod = "Unknown"
}// **Switch statement: Compound cases** 
let service = "Seamless"

switch service {
  case "Uber", "Ola":
    print("Travel")
  case "Swiggy", "Zomato", "Uber Eats":
    print("Restaurant delivery")
  case "Grophers", "Big Basket":
    print("Grocery delivery")
  default: 
    print("Unknown service")
}// **Switch statement: Where clause** 
let num = 7

switch num {
  case let x where x % 2 == 0:
    print("\(num) is even")
  case let x where x % 2 == 1:
    print("\(num) is odd")
  default:
    print("\(num) is invalid")
}
```

默认情况下，Swift 中的语句不会穿过每个案例的底部进入下一个案例。相反，只要第一个匹配的`switch`案例完成，整个`switch`语句就结束执行，而不需要显式的`break`语句。

## 守卫声明

guard 语句可以作为 if else 语句的替代语句，也可以与 optionals 一起使用。

```
// **As an alternative to if else**
guard number > 5, number > 4 else { return false }
return true// **Using guard with the Optionals** var text: String? = nil
text = "some text"fun print() {
   guard let valueText = text else { return } 
   print(valueText)
}
```

# **收藏类型**

Swift 提供了三种主要的*集合类型*，称为数组、集合和字典，用于存储值集合。数组是值的有序集合。集合是唯一值的无序集合。字典是键值关联的无序集合。

```
// **Declaring an empty string array**
let array: [String] = [String]()
print("size of array: \(array.count))// **Declaring a new Dictionary** let array = [String: String]()
 // when you fetch a element from dictionary it is optional as the              //value may or may not be present for the key.
```

# 环

```
**for loop** // **iterating over an array**
for item in array {
   print(item)
}**// iterating over a range**
for index in 0..5 {
   print(index)
}**while loop**var count = 5
while count > 1 {
   print(count)
   count-=1
}
```

# 功能

```
**Function syntax**
func functionName(parameter: ParameterType) -> ReturnType {
// Body of the function
}**Example** func addTwoNum(num1: Int, num2: Int) -> Int {
  return num1 + num2
}// calling this function
let sum = addTwoNum(num1: 10, num2: 20)// **using parameter labels in function**
func addTwoNum(using num1: Int, and num2: Int) -> Int {
  return num1 + num2
}
//calling of function
let sum = addTwoNum(using: 10, and: 20)
// this makes reading function more like natural english
```

# 类和结构

> ***结构*** *是值类型，而* ***类*** *是引用类型。*如果你将两个变量指向同一个结构，它们有自己独立的数据副本。对于对象，它们都指向同一个变量。

```
**class** Car {
let make: String
let color: String
  init(make: String, color: String) {
    self.make = make
    self.color = color
  }
}// Instantiating an object of the above class
var bmw = Car(make: "BMW", color: "white")// Classes need initialisers to initialize the values.**struct** Car {
let make: String
let color: String
  init(make: String, color: String) {
    self.make = make
    self.color = color
  }
}// Instantiating an object of the above class
var bmw = Car(make: "BMW", color: "white")
```

**Swift 中的类和结构(structs)非常相似，以至于一开始很容易混淆，但实际上有一些重要的潜在差异:**

*   结构不能从另一种结构继承，而类可以在其他类的基础上构建。
*   您可以使用类型转换在运行时更改对象的类型。结构不能继承，所以只能有一种类型。
*   如果你将两个变量指向同一个结构，它们有自己独立的数据副本。对于对象，它们都指向同一个变量。

最后一点特别重要:通过 struct，你知道你的数据是固定的，就像一个整数或其他值。这意味着如果你把你的结构传递给一个函数，你知道它不会被修改。

# 枚举

一个*枚举*为一组相关值定义了一个公共类型，并使您能够在代码中以一种类型安全的方式使用这些值。

```
enum CompassPoint {
   case north
   case south 
   case east 
   case west 
}var directionToHead = CompassPoint.west
directionToHead = .east// **Multiple cases can appear on a single line, separated by commas:**
enum Planet {
   case mercury, venus, earth, mars, jupiter, saturn, uranus,     neptune
}// **We can use values with enums** enum MoveDirection : String {
    case forward = "you moved forward"
    case back = "you moved backwards"
    case left = "you moved to the left"
    case right = "you moved to the right"
}var action = MoveDirection.left;
print(action.rawValue) 
// this will print out "you moved to the left"
```

# 协议

一个*协议*定义了适合特定任务或功能的方法、属性和其他需求的蓝图。然后，类、结构或枚举可以*采用*该协议，以提供这些需求的实际实现。任何满足协议要求的类型被称为*符合该协议的*。

例子:如果你想成为一个人，你必须吃饭，工作，睡觉！！！。

*   协议可以具有符合该协议的类、枚举或结构可以实现的属性和方法。
*   协议声明仅指定所需的属性名称和类型。它没有说明属性应该是存储的还是计算的。
*   协议还规定每个属性是否必须是可获取的或者可获取的*和可设置的*。
*   属性需求总是被声明为变量属性，以关键字`var`为前缀。
*   可获取和可设置属性通过在它们的类型声明后写`{ get set }`来表示，可获取属性通过写`{ get }`来表示。

## 协议扩展

可以扩展协议，为符合的类型提供方法和属性实现。这允许您定义协议本身的行为。

```
protocol SomeProtocol {
   var data: String { get set }
   fun displayData()
}struct SomeClass: SomeProtocol {
   var data: String 
   init(data: String){
      self.data = data
   }
   fun displayData(){
      print(data)
   }
}
// **A class can conform to zero or more protocols**
```

数据源和代理是 iOS 中的协议。数据源是数据相关的，委托是用户交互功能相关的协议。

# **强引用和弱引用**

默认情况下，创建的变量是**强**变量。

为了确保实例在仍然需要时不会消失，ARC(自动引用计数)跟踪有多少属性、常数和变量当前引用了每个类实例。只要仍然存在至少一个对实例的活动引用，ARC 就不会释放该实例。

为了实现这一点，每当您将一个类实例分配给一个属性、常量或变量时，该属性、常量或变量会使实例的 ***强*** *引用*。该引用被称为“**强**”引用，因为它牢牢地控制着该实例，并且只要该强引用存在，就不允许释放该实例。

**弱**和**无主**引用使引用循环中的一个实例能够引用另一个实例*，而*不会牢牢地控制它。因此，当弱引用仍在引用该实例时，该实例有可能被释放。当它引用的实例被释放时，ARC 自动设置一个弱引用给`nil`。而且，因为弱引用需要允许它们的值在运行时被更改为`nil`，所以它们总是被声明为可选类型的变量，而不是常量。

## 弱小和无主的区别

弱参考可以设置为`nil`，它总是被声明为可选的。但是如果一个无主引用的被引用实例被释放，它不会被设置为`nil`。因此，如果访问无主引用的被引用对象，将引发致命错误。

# 关闭

*闭包*是自包含的功能块，可以在代码中传递和使用。Swift 中的闭包类似于其他编程语言中的 lambdas。

```
// **Closure Syntax**
{
   (parameters) −> return-type in
   statements
}var isGreaterThanThree: ( (Int)->(Bool) ) = { number in
    if number > 3
       return true
    else
       return false
}
// **Since the closure is a var it can be reassigned**
```

默认情况下，闭包是非转义的。

## 1.@非转义闭包:

当将闭包作为函数参数传递时，无论何时调用，闭包都会与函数体一起执行。当执行结束时，传递的闭包会超出范围，不再存在于内存中。

## 2.@转义闭包:

当将闭包作为函数参数传递时，闭包会被保留以便以后执行，这样函数的主体就会被执行并返回。当执行结束时，传递的闭包的范围存在，并且存在于内存中，直到闭包被执行。

# Typealias

类型别名允许您为程序中现有的[数据类型](https://www.programiz.com/swift-programming/data-types)提供一个新名称。在声明了类型别名之后，可以在整个程序中使用别名来代替现有的类型。

类型别名不会创建新类型。它们只是为现有类型提供了一个新名称。

`typealias`的主要目的是使我们的代码可读性更强，在人类理解的上下文中更清晰。

```
typealias name = existing type
```

在 Swift 中，大多数类型都可以使用`typealias`。它们可以是:

*   **内置类型**(例如:String，Int)
*   **用户定义的类型**(例如:类、结构、枚举)
*   **复杂类型**(例如:闭包)

## 内置类型的 Typealias

```
typealias StudentName = Stringlet name:StudentName = "Jack"
```

## 用户定义类型的 Typealias

```
// **without typealias** class Student {

}
var students:Array<Student> = [] // **using typealias** typealias Students = Array<Student>
var students:Students = []
```

## 为闭包键入别名

```
// **without typealias**
func someMethod(oncomp:(Int)->(String)){

}// **using typealias** typealias CompletionHandler = (Int)->(String)
func someMethod(oncomp:CompletionHandler){

}
```

# 扩展ˌ扩张

*扩展*为现有的类、结构、枚举或协议类型添加新的功能。这包括扩展您无法访问原始源代码的类型的能力。

```
extension Int {
   var add: Int {return self + 100 }
   var sub: Int { return self - 10 }
   fun mul(num: Int): Int { return self * num }
   fun div(num: Int): Int { return self / num}
}
```

Swift 中的扩展可以:

*   添加计算实例属性和计算类型属性
*   定义实例方法和类型方法
*   提供新的初始化器
*   定义和使用新的嵌套类型
*   使现有类型符合协议

# 错误处理

Swift 为运行时抛出、捕捉、传播和操纵可恢复错误提供了一流的支持。

下面是一个`do` - `catch`语句的一般形式:

```
**do** {
    **let** encrypted = **try** encrypt("secret information!", withPassword: "")
    print(encrypted)
} **catch** EncryptionError.empty {
    print("You must provide a password.")
} **catch** EncryptionError.short {
    print("Passwords must be at least five characters, preferably eight or more.")
} **catch let err** {
    print("Something went wrong! \(err)")
} // **Throwing Errors in Initializers**enum StudentError: Error {
  case invalid(String)
  case tooShort
} class Student {
  var name: String?     
  init(name: String?) 
  throws {
  guard let name = name else {
     throw StudentError.invalid("Invalid")
  }
  self.name = name }
```

# 访问控制

*访问控制*限制其他源文件和模块中的代码对您的部分代码的访问。此功能使您能够隐藏代码的实现细节，并指定可以通过其访问和使用代码的首选接口。

# 开放和公开(限制最少)

允许在定义模块(目标)之外使用实体。像开放访问级别一样，公共访问级别允许在定义模块(目标)之外使用实体。

> **但是开放访问级别允许我们从另一个模块中子类化它，而在公共访问级别，我们只能从定义它的模块中子类化或覆盖它。**

# 内部(默认访问级别)

> **内部是默认的访问级别。内部类和成员可以在定义它们的同一个模块(目标)中的任何地方被访问。**

当定义一个应用或框架的内部结构时，我们通常使用`internal` access。

# 文件私有

将实体的使用限制在其定义源文件中。当在整个文件中使用这些细节时，我们通常使用`fileprivate` access 来隐藏特定功能的实现细节。即使用`fileprivate`访问级别定义的功能只能从定义该功能的 swift 文件中访问。

# 私有(最严格)

> **私有访问将实体的使用限制在封闭声明以及同一文件**中该声明的扩展上。

我们通常使用`private` access 来隐藏特定功能的实现细节，当这些细节只在一个声明中使用时。

希望你学到了一些东西！感谢您的阅读。

你可以在 [***推特***](https://twitter.com/adilkhanforeal) **和**[***insta gram***](https://www.instagram.com/adilkhanforeal/)上找到我

[](https://www.linkedin.com/in/iadilkhan/) [## Adil Khan - BBD 大学-印度北方邦勒克瑙| LinkedIn

### 在世界上最大的职业社区 LinkedIn 上查看 Adil Khan 的个人资料。Adil 有 1 个工作列在他们的…

www.linkedin.com](https://www.linkedin.com/in/iadilkhan/) 

*点击*👏表示你的支持，并与其他媒体用户分享。