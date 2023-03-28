# JavaScript 基础入门指南

> 原文：<https://medium.com/geekculture/a-beginner-guide-to-javascript-fundamentals-e9c0aecdc652?source=collection_archive---------37----------------------->

![](img/739799257072f3f90c9774283d3d8c29.png)

Javascript Fundamentals

Javascript 是一种面向对象的编程语言。除了原始数据类型，javascript 中的所有东西都是对象。

*数据类型有两种:*
1。原始数据类型
2。目标

## **1。原始数据类型**

在所有编程语言中，原语都是简单的元素。Javascript 中有六个原始值可用。它们是:字符串、数字、布尔值、空值、未定义、符号。这个列表还包括数组和日期，所有这些在 javascript 中都被算作对象。

**布尔类型:**值:true、false
**Null 类型:**值:null
**未定义类型:**值，未定义
**数字类型:**值:18437736874454810624 有限数字
**字符串类型:**值:16 位无符号整数值
**符号类型:**值:唯一且

## 2.目标

所有像数组、函数、对象、日期和数字/字符串/布尔的包装函数这样的东西，除了上面提到的隐式数据类型，都是对象。隐式数据类型和对象之间的主要区别在于内存中的存储系统。

原始数据类型将值存储在自身中，其中对象不直接存储值，而是将值保存在另一个地方，并且对象只有该值的引用。

```
var x= 20, y= 30;console.log('Before: Value of x: ' + x+ ' and value of y: ' + y);function swap(x, y) {
   console.log('Before swap: Value of x: ' + x+ ' and value of y: ' + y);
   let temp = x;
   x = y;
   y = temp;
   console.log('After Swap: Value of x: ' + x+ ' and value of y: ' + y);
}swap(x, y);
console.log('After: Value of x: ' + x+ ' and value of y: ' + y);
```

## 3.表示

表达是表达一个人的思想或感情的行为。

每个表达式是一个代码单元。其确定特定代码的值。这意味着，表达实际上产生价值。javascript 中有几种类型表达式。我们将讨论它们。

javascript 中的表达式有:

a.算术表达式

b.串表达式

c.数组和逻辑表达式

d.逻辑表达式

e.左侧表达式

f.属性访问表达式

**a .算术表达式**

所有的数学运算都称为算术表达式。

```
6/2
i--
i+= 2
```

串表达式

```
'Learning ' + 'Javascript' === 'Learning Javascript'; // true
```

数组和逻辑表达式

```
[] // Array Literals
[1,2,3] // Arrayx && y //Logical expression
x || c //Logical expression
```

## 4.试着接住

Try catch 是用来找出并处理代码中容易出错的部分的测试方法。在复杂代码中使用 try-catch 语句是一个好习惯。通常 catch 语句是在 try 语句之后声明的。当然，一个 try 块可以有多个 catch 块。

```
try {
    const x = [1,2,3,4,5,6]; //x is an array
    document.write(x); // displays elements of x
    document.write(y); // b is undefined
} catch(e) {
    alert('There is an error which is' + e);
}
```

## 4.块绑定

实际上绑定意味着变量。当我们声明一个变量时，我们将值绑定到一个被定义为变量的名字上。并且该值被绑定在一个范围内。

因此，每当我们用 *var、const* 和 *let 声明任何变量时，就会发生绑定。*

最常见的块是:

**如果阻塞**

```
if(condition){
    const demoText = 'Javascript';
    console.log(demoText);
}
```

**否则阻止**

```
else{
    console.log(demoText);//demoText has a value of undefined
}
```

**封闭块**

```
function demoFunction(condition){ //previous blocks console.log(demoText);//demoText has a value of undefined
}
```

这里发生了什么？如果我们在 demoFunction 中将 true 作为布尔值传递，函数将从 If 获得输出。如果我们在函数中将 false 作为布尔值传递，那么函数将从 else 获得输出。发生这种情况是因为吊装。

## 5.提升

我们不需要做任何提升的工作。这是 javascript 的默认行为。因为这是默认行为，所以所有函数都在创建阶段被提升。因此，我们可以在函数声明之前调用它。

```
demoFunction();function demoFunction() {
   var x= 20;
   var y= 40;
   var sum = x+ y;
   console.log('Sum of the Numbers: ' + sum);
}// Sum of the Numbers: 60
```

但是，函数表达式不会以这种方式出现。因为在函数表达式中，我们将函数存储在变量中。并且该功能在创建阶段被设置为未定义。如果我们在声明之前调用函数表达式，它将会抛出错误，因为原始函数是稍后执行的。

```
demoFunction2();var demoFunction2= function() {
   console.log('Learning Javascript is ');
}// Uncaught TypeError...
```

## 6.块级声明

在给定块外部不可访问的声明称为块级声明。

我们可以用 *var 代替*字母*。*有趣的是 let 没有被提升到顶部，所以我们可能想把它放在块的顶部。所以它们对整个街区都是可用的。

```
function getName(condition) {

    if (condition) {
        let name = "Jamil";

        // ....

        return value;
    } else {

        // value is not available

        return null;
    }

    // value is not available
}
```

## 7.箭头功能

Arrow 函数只不过是 javascript 常规函数的一个简短形式。我们可以用它来缩短常规函数，这样就减少了样板代码。

```
const fruits = [“mango”, “guava”, “pineapples”]; 
const allFruits= fruits.map(fruit => { return `${fruit}`;}); console.log(allFruits);
```

这样，我们必须编写一些代码来完成这项工作。

## 8.传播算子

Spread operator 是 ES6 中 javascript 的一个巧妙概念。但是大多数人不想使用它，因为他们认为它有点复杂。老实说，这是一种句法糖。

## **9。Javascript 注释:**

javascript 中的注释用于解释代码，增加了开发人员的可读性。Javascript 不会执行注释中的任何内容。javascript 中有两种类型注释。这些是单行注释和多行注释。多行注释通常用于正式文档。

单行注释:

```
//This is single-line commentdocument.getElementById(“text1”).innerHTML = “Some Text”;
```

多行注释:

```
/****************This is multi-line comment***************/
document.getElementById(“text2”).innerHTML = “Some more text”;
```

## 10.贮藏

缓存只不过是一个提供数据可用性工具的概念。有时客户需要频繁地请求常用数据。为了减少时间消耗，通常使用的数据被存储，从而提高效率。