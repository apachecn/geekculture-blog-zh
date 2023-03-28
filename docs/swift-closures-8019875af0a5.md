# Swift|关闭

> 原文：<https://medium.com/geekculture/swift-closures-8019875af0a5?source=collection_archive---------6----------------------->

## 快速关闭指南

![](img/f51431bf8fbe4267d0dd0cc30abe0663.png)

Photo by [Joan Gamell](https://unsplash.com/@gamell?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/functions?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

闭包是赋给变量的函数。这些变量用于调用函数。让我们从一个简单的打印消息的例子开始。

```
let study = {
    print("I am studying")
}
```

上面是一个没有参数的简单闭包。您可以通过调用 study()来调用这个闭包。

# 什么是闭包，为什么在 Swift 中使用闭包？

闭包主要用于存储函数。就像我之前提到的，闭包允许你将一些函数封装到一个变量中，并将它们存储在某个地方。闭包在第一次学习 Swift 时很难读懂，尤其是在接受和返回自己的参数时。但是如果你经常看到闭包，你会习惯的。

# 在闭包中调用参数

在闭包中，参数列在花括号{}内。如果您想让闭包接受参数，就在大括号{}后面的括号()中添加它们。

例如，您可以创建一个将年龄作为参数的闭包。

```
let person = { (age:Int) in 
     print("This person is (\(age)) years old" 
}
```

用参数调用闭包看起来像下面这样

```
person(11)
//output 
//"This person is 11 years old"
```

# 为什么闭包在花括号{ }里面？

让我们看看函数和闭包之间的区别

操作码

```
func pay(user:String, amount: Int){ 
    //code
}
```

关闭代码

```
let payment = { (user: String, amount: Int) in
    // code
}
```

您可能已经注意到，参数已经被移到了花括号内。中的关键字**标志着闭包主体的开始。参数和闭包体是同一个代码块的一部分，并且是变量，所以如果{}中有参数，它们表示闭包类型。在中查找关键字，以区分参数和正文。**

# 从闭包返回值。

闭包也可以返回值，类似于参数。return 方法就写在。让我们来看一个例子，让事情更清楚。

```
let eating = { (food:String) -> String in 
    return "I am eating \(food)"
}
```

因为我们希望闭包返回一个字符串，而不是直接输出消息，所以我们需要像下面这样声明返回类型->String。不要；忘记 in 关键字跟在返回类型后面。

```
let lunch = eating("pizza") 
print(lunch)//OUTPUT 
//"I am eating pizza"
```

# 返回不带参数的值

只需添加一个空参数，括号内没有值。

```
let eatPizza = {() -> String in 
     return "I am eating pizza"
}
```

# 将闭包传递给函数

闭包可以像其他数据类型一样使用，所以可以在函数中传递它们。

让我们从简单的开始

接受闭包的函数

```
//Function that takes in Closure 
func getReady(action:() -> Void) { 
    print("I wake up and wash my face") 
    action() 
    print("Ready!")
}
```

关闭:

```
let brushTeeth = {
     print("I brush my teeth") 
}
```

呼叫功能

```
getReady(action: brushTeeth) 
//OUTPUT 
//"I wake up and wash my face"
// "I brush my teeth" 
// "Ready!"
```

# 尾随闭包

让我们尝试一种不同的方法来调用上面的函数，不要将闭包保存在不同的变量中。在这个例子中，我们将把一个字符串作为闭包的参数。

```
func getReady(action:(String) -> Void) {
    print("I wake up and wash my face") 
    action("Brush my Teeth") 
    print("Ready!")
}
```

注意我们是如何向闭包传递一个字符串的。正如参数中所示，这就是我们需要做的。我们现在将调用该函数，并告诉用户在调用该函数时闭包必须做什么。

```
getReady{(thingTodo:String) in 
     print("I will \(thingTodo)")
}OUTPUT 
// "I wake up and wash my face"
// "I will Brush my Teeth"
// "Ready!"
```

进展顺利吗？？

让我们尝试一个更难的例子

```
func getDirections(to destination: String, then travel:(([String]) -> Void){   print("Will be taking vehicle")   
   let directions = [
        "Go straight ahead",
        "Turn left at the corner", 
        "When you see the school go straight ahead",
        "Then you will arrive at \(destination)"]
   travel(directions)
}
```

请记住，您需要在初始化方向列表后调用闭包，并将那个[String]作为闭包的参数调用。

让我们用结尾闭包来调用函数

```
getDirection(to:"HOSPITAL") { (path:[String]) in  
    for i in path{
        print(i) 
    }
}
//OUTPUT 
// "Driving with Vehicle"
// "Go straight ahead"
// "Turn left at the corner", 
// "When you see the school go straight ahead",
// "Then you will arrive at HOSPITAL"
```

# 返回值时使用闭包作为参数

Swift 中的闭包可以带参数，也可以带返回值，您可以在函数中使用这些闭包。更好的是，这些函数还可以返回值。

```
**func** MorningRoutine(first:String , second:((String) -> String)){
     print("My Morning Routine")
     print(first)
     **let** secondThing = second("Brush my Teeth")
     print(secondThing)
     print("I am finished with my morning routine")}
```

注意我们是如何在参数中将一个字符串返回给闭包的？当我们在函数中调用闭包时，我们将把它的值保存到一个常量中，以后再调用它。

让我们试着调用这个函数，看看闭包是什么样子的。

```
MorningRoutine(first:"Wake Up "){(doThis: String) -> String **in
    return** doThis
}//OUTPUT //"My Morning Routine"//"Wake Up"//"Brush my Teeth"//"I am finished with my morning routine"
```

# 具有两个返回值的闭包的函数

```
**func** MorningRoutine(first:String , second:((String) -> String)) -> String {
     print("My Morning Routine")
     print(first)
     **let** secondThing = second("Brush my Teeth")
     print(secondThing)
     print("I am finished with my morning routine") 

     return("GO TO WORK NOW") }
```

返回函数

```
print(MorningRoutine(first:"Wake Up "){(doThis: String) -> String **in
    return** doThis
})
//OUTPUT
//"My Morning Routine"
//"Wake Up"
//"Brush my Teeth"
//"I am finished with my morning routine"
//"GO TO WORK NOW"
```

本节到此为止！闭包一开始可能很难理解，但是一旦你练习了，你就会掌握它的窍门了！如果你有任何问题，请发邮件到 yu24c@mtholyoke.edu 找我