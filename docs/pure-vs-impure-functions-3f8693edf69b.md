# 纯函数与不纯函数

> 原文：<https://medium.com/geekculture/pure-vs-impure-functions-3f8693edf69b?source=collection_archive---------6----------------------->

![](img/5326647cb59138a53eeeffeb0a5d281d.png)

pure vs impure functions

这是你在编程语言中经常听到的两个术语，叫做**纯**和**不纯函数**。

你知道`Pure Function`总是依赖于参数，不应该有任何副作用。

# 什么叫副作用？☠️

当你试图在函数中使用外部代码，或者你正在改变一个变量，那么它会产生一个副作用。

示例:

```
let num = 10
function mul(val) {
    return num += val  
}
```

正如您在上面的代码片段中看到的，`mul`函数依赖于一个名为`num`的外部变量。还有，变异`num`值，这使得它的功能不纯。

让我们再看一个简单的例子:

```
function hello() {
  console.log("Hello Folks!");
}
```

那么，你对上面的片段怎么看？🤔

.

.

.

耶！这也是一个不纯的函数😵‍💫

众所周知，JavaScript 是同步的，使用“控制台”或任何“回调”或“承诺/获取”将使函数异步。

这里使用控制台，这是一个 Web API，使它成为一个不纯的函数。

让我们用**例子**来看看其他副作用:

1.  DOM 操作，任何回调或读/写文件

```
function mul(a,b) {
  document.write("Started Multiplication")
  return a * b
}
console.log(mul(2,5))
```

2.更新或修改纯函数中的全局变量

```
let x = 10function mul(a,b) {
    document.write("Started Multiplication")
    return a * b * x
}
console.log(mul(2,5))
```

3.同样，变异的外部变量依赖于一个外部变量。

```
let xfunction mul(a,b) {
    x = 10
    return a * b * x
}
console.log(mul(2,5))
```

让我们来理解纯函数和不纯函数，因为现在你对副作用有了一个概念

# 纯函数

*   如果传递了相同的参数，它总是返回相同的结果
*   在程序执行期间，它从不依赖于任何状态/数据/变化
*   它总是会返回一些东西
*   在这里，编写测试用例将会很简单

# 不纯函数

*   改变已传递的任何参数的内部状态
*   它可能在不返回任何内容的情况下生效
*   编写测试用例会有点复杂，因为可能会有副作用

# 纯方法和不纯方法

这些是纯粹的方法:

*   Array.map()
*   Array.reduce()
*   Array.filter()
*   Array.concat()
*   …(扩展语法，主要用于创建副本)

这些是不纯的方法:

*   Array.splice()
*   数组.排序()
*   Date.now()
*   Math.random()

# 加分🔖

您可以将一个外部状态克隆到一个纯函数中

将外部状态克隆到纯函数中并不会使函数不纯。

示例:

```
let name = ["suprabha", "supi"]function fullName(newName, name) {
  let clonedName = [...name]
  clonedName[clonedName.length] = newName
  return clonedName
}console.log(fullName("sumi", name)) // ['suprabha', 'supi', 'sumi']
```

在上面的代码片段中，您在`fullName`函数中使用了`...`扩展运算符。它将创建它的一个克隆，然后更新它。所以，它不依赖于`name`变量。

感谢您阅读帖子！

希望这篇文章对你有用。如果你有任何问题，欢迎在评论区提出，或者在这里联系我👇

🌟[推特](https://twitter.com/suprabhasupi) |📚[电子书](https://gum.co/css-pseudo-class-elements) |🌟 [Instagram](https://www.instagram.com/suprabhasupi/)