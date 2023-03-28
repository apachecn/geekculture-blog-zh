# 每个开发人员都应该知道的 10 个 JavaScript 概念。

> 原文：<https://medium.com/geekculture/10-javascript-concepts-that-every-developer-should-know-702330e662e2?source=collection_archive---------2----------------------->

![](img/faa3deb0f45942bb545604ae02698e39.png)

JavaScript ES6

嗨！程序员们，今天我将探讨每个开发者都应该知道的 10 个 javaScript 概念。所以，我们走吧…

# **1。箭头功能**

一个**箭头函数表达式**是声明传统函数表达式的另一种方式。但它与传统函数有所不同。

1.  传统函数有'**这个'**关键字，但是箭头函数没有'**这个'**关键字。
2.  传统函数有'**参数'**关键字，但箭头函数没有'**参数'**关键字。
3.  传统函数可以用作'**构造函数** ' ，但是箭头函数不能用作'**构造函数**。

如果我们使用一个箭头函数，我们应该删除函数关键字，并在自变量和开始体括号之间设置**箭头= >** 。我们可以将函数存储在变量中。如果一个箭头函数写在一行中，那么我们可以删除**主体括号**和**返回**关键字。如果 arrow 函数只有一个参数，那么我们可以去掉**参数括号**。

**代码示例:**

```
const sum = (a, b)=> {
  return a+b;
}
console.log(sum(5, 7)) *// output, 12*const multiply = (a, b) => a*b 
console.log(multiply(5, 7)) *// output, 35*const double = a => a*2
console.log(double(5)) *// output, 10*
```

# **2。**默认参数

您可以在 es6 之后的函数中使用**默认参数**。如果从不传递参数，那么您可以设置一个默认参数。如果您传递了 undefined 或者您没有传递该参数，则默认参数将被设置，否则它不会被设置。

**代码示例:**

```
function add(a, b=5){
  return a+b;
}
console.log(add(7)) *// output, 12*function add(a, b=5){
  return a+b;
}
console.log(add(7, 4)) *// output, 11*
```

# **3。生命功能**

**life**完整含义— **立即调用函数表达式**

如果我们定义了一个函数，我们希望这个函数会立即调用，那么我们可以使用 IIFE 函数。IIFE 函数是一个匿名函数，该函数将立即调用。我们不能下次再调用生命函数了。

**代码示例:**

```
(function (){
  console.log(5+7); 
})(); *// output, 12*
```

# **4。**传播算子

**展开运算符** (…)或三个点是 es6 的特征。您可以使用 spread 运算符连接数组、对象和字符串。它用于要展开的数组表达式或字符串，或者要就地展开的对象表达式。您可以复制数组的所有元素、对象的所有属性以及字符串的所有 iterable。

**代码示例:**

```
const numbers = [1, 8, 5, 15, 10];
console.log([...numbers]) *// output,* [1, 8, 5, 15, 10]
console.log([...numbers, 65]) *// output,* [1, 8, 5, 15, 10, 65]const user = {name: 'Shuvo'}
console.log({...user}) *// output,* {name: 'Shuvo'}
console.log({...user, id: '1'}) *// output,* {name: 'Shuvo', id: '1'}
```

# 5.isNaN()方法

**isNaN()** 方法如果自变量为 NaN 则返回 **true** ，否则返回 false。

**代码示例:**

```
console.log(isNaN(12)); *// output, false*
console.log(isNaN("false")); *// output, true* console.log(isNaN("12")); *// output, false* console.log(isNaN("")); *// output, false* console.log(isNaN("12as")); *// output, true*
```

JavaScript 有两种数据类型。**原语**和**引用**或*对象和函数。*

# **6。原始值**

有 7 种原始数据类型。他们是 ___

1.  数字
2.  线
3.  布尔代数学体系的
4.  不明确的
5.  空
6.  标志
7.  BigInt

**代码示例:**

```
console.log(typeof(5)) //output, "number"
console.log(typeof('Hello')) //output, "string"
console.log(typeof(true)) //output, "boolean"
console.log(typeof(undefined)) //output, "undefined"
console.log(typeof(null)) //output, "object"**In JavaScript, typeof null is "object". We can consider it a bug in JavaScript and we can think it should be "null".
```

# 7.**引用**或*对象和功能*

没有原语数据类型，javaScript 的其他数据类型都是引用数据类型。他们是 ___

1.  目标
2.  功能

数组、正则表达式和日期是对象类型。

**代码示例:**

```
console.log(typeof({name: 'Faysal'})) //output, "object"
console.log(typeof([1, 2, 5])) //output, "object"
console.log(typeof(() => 4+4)) //output, "function"
console.log(typeof(/exp/)) //output, "object"
```

# 8.双倍相等(==)对三倍相等(===)

在 javaScript 中，两个 double equal 比较值。如果值相等，则返回**真。**而是三重相等的比较值和比较数据类型。如果值相等，类型相等，则返回 true，否则返回 false。我们应该使用三重相等来比较，这是最佳做法。

**代码示例:**

```
console.log(5=="5") *// output, true* console.log(5==="5") *// output, false* console.log(1==true) *// output, true* console.log(1===true) *// output, false* console.log(0==false) *// output, true* console.log(0===false) *// output, false*
```

# 9.三元运算符

三元运算符，条件检查的另一种方式。这是条件的最小方式。您可以使用三元运算符编写一行条件。在使用问号(？)和冒号(:)。首先我们写出条件， `num>5` 然后我们要用一个问号(？)，那么如果条件为真，我们连接代码，然后我们应该使用冒号(:)，如果条件为假，我们连接代码。我们可以在三元运算符中使用嵌套条件。三元运算符应该写出条件为真和假的代码。

**代码示例:**

```
const number1 = 5;
const number2 = 8;
const largest = number1 > number2 ? number1: number2;
console.log(largest) *// output, 8*
```

# 10.解构

有两种析构，数组和对象。

**对象析构:**我们可以析构变量中的对象属性，变量名和属性名应该相同。我们可以析构一个对象的任何属性。在对象析构中，我们不应该保持任何顺序，即属性是第一个或最后一个或任何位置。在对象析构中，我们应该用花括号`{ }`声明一个变量。在花括号中，我们应该写那些我们想从对象中析构的属性。然后我们应该使用赋值操作符`=`，在右边，我们使用那个对象。

**数组析构:**我们可以析构变量中的数组元素，变量名和元素名不需要相同。在数组析构中，我们应该保持元素在第一个、最后一个或任何位置的顺序。在数组析构中，我们应该用数组符号`[]`声明一个变量。在数组符号中，我们应该按照元素在第一个、最后一个或任何位置的顺序来写元素。如果我们想要第二个元素而不想要第一个元素，我们可以只用逗号。然后我们应该使用赋值操作符`=`，在右边，我们使用那个数组。

**在数组和对象析构中，我们可以使用扩展运算符或三点来析构数组变量中的另一个元素数组和对象变量中的另一个对象属性。

**代码示例:**

```
*// array* destructuring
const [num1, num2] = [5, 10];
console.log(num1, num2) *// output, 5, 10* const [, num2] = [5, 10];
console.log(num2) *// output, 10* const [num1, ...other] = [5, 10, 15];
console.log(num1, other) *// output, 5, [10, 15]**// object* destructuring
cosnt {num1, num2} = {num1:5, num2:10};
console.log(num1, num2) *// output, 5, 10* const {num2, num1} = {num1:5, num2:10};
console.log(num2, num1) *// output, 10, 5* const {num2} = {num1:5, num2:10};
console.log(num2) *// output, 10* const {num1, ...other} = {num1:5, num2:10, num3: 56};
console.log(num1, other) *// output, 5, {*num2:10, num3: 56*}*
```