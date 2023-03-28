# 理解 JavaScript 现代语法和一些功能

> 原文：<https://medium.com/geekculture/understand-javascript-modern-syntax-and-some-functionality-7cf9a079aca8?source=collection_archive---------35----------------------->

![](img/ba22a842df939f344b63f978a2ae4045.png)

## 1.默认参数

一般来说，JavaScript 中函数参数的默认值是未定义的。注意下面的例子:如果在这里进行 sum 调用时没有给出 num2 的任何值，sum 函数中的`num2 = undefined`和`num1 + num2`将给出`sum = NaN`

```
function sum(num1, num2){
  return num1 + num2;
}console.log(sum(25)) // NaN
console.log(sum(25, 5)) // 30
```

但是函数的参数可以在 ES6 中默认设置。将`num2 = 0`设置为默认值，如下所示:

```
function sum(num1, num2 = 0){
  return num1 + num2;
}console.log(sum(25)) // 25
console.log(sum(25, 5)) // 30
console.log(sum(5, undefined)) // 5
```

## 2.扩展运算符(…)

当我们在 javaScript 代码中看到`…`时，它要么是 rests 参数，要么是 Spreed 操作符。

Spread 运算符可以传递许多参数来调用数组列表中的函数。

```
function sum(num1, num2, num3){
  return num1 + num2 + num3;
}const number = [10, 20, 30];
console.log(sum(...number)); // 60
```

当我们复制数组或对象时。在下面的例子中，`num1`的值是从`num1`数组复制到`num2`中的，并没有改变`num1`数组。只是抄袭了`num1`。

```
const num1 = [10, 20, 30];
const num2 = [...num1, 25, 36]console.log(num2) // [ 10, 20, 30, 25, 36 ]
```

## 3.箭头功能

箭头函数是非常简单的创建函数。它处理单行语法来创建函数。而且当它使用单行语法时，它不使用花括号，也不使用 return。当它不传递任何参数时，看起来像这样:

```
const name = () => 'Hossain Rabbi';console.log(name()) // Hossain Rabbi
```

如果它只传递一个参数，就不能使用括号。

```
const multiply = x => x * x;console.log(multiply(5)) // 25
```

如果它再传递一个参数，它使用圆括号，当它使用多行时。它给出了花括号和 returns 语句。

```
const sum = (a, b) => {
  return a + b
};console.log(sum(20, 5)) // 25
```

## 4.评论

注释可以是单行和多行，单行开始:`//`和多行:`/*...*/`

单线:

```
// this is single line comment
```

多行:

```
/*
  const a = 20;
  const b = 25;

  console.log(a + b);
*/
```

## 5.错误处理，“尝试…捕捉”

不管我们编程有多棒。有时我们的代码出错，用户的意外输入，以及错误的服务器响应。但是`try...catch`语法很容易处理来自 JavaScript 代码的错误。

*   首先，执行代码`try{...}`。
*   如果代码没有错误，则`catch`被忽略，
*   如果代码出错，则`try`执行停止，并转到`catch(err)`块。`err`对象详细信息有误。

如果没有错误。

```
const a = 66;
const b = 4;try{
  console.log(a + b); // 70
}catch(err){
  console.log(err); 
}
```

如果错误。

```
const a = 66;try{
  console.log(a + b);
}catch(err){
  console.log(err) // Error: b is not defined
}
```

## 6.编程风格

我们的代码必须尽可能简单和干净。

花括号:javaScript 项目中最常用的花括号。开始是大括号，然后是一些行代码，最后是大括号。在这种情况下:在一个空格前使用左大括号。

```
if (condition) {
  // something...
}
```

如果一行`if`阻塞，不需要用花括号。在这种情况下，不要使用第二条线来控制。

```
const a = 10;if (a) console.log(a);
```

最好的变体是:

```
const a = 10;if (a) {
  console.log(a);
}
```

这里给你一些 javaScript 风格指南，你可以任意选择:

*   谷歌风格指南
*   [Airbnb 风格指南](https://github.com/airbnb/javascript)
*   [惯用的。JS](https://github.com/rwaldron/idiomatic.js)
*   [标准](https://standardjs.com/)

## 7.块级声明

块级声明是在函数或块`{}`内声明变量。

```
function myName() {
  /* 
  This name is block level variable. 
  inside of a block {} 
  */
  const name = 'Hossain';
  return name;
}console.log(myName()); // Hossain
```

## 8.循环中的块绑定

当我们用`var`变量声明一个`for`循环时，这个变量在循环之外使用。

```
for(var i = 0; i < 5; i++){
  console.log(i);
}
console.log(i); // 5
```

但是其他编程语言中，`for`循环默认在块级范围内。在这种情况下，如果我们在`for`循环中使用`let`变量。它是块级范围。

```
for(let i = 0; i < 5; i++){
  console.log(i);
}
console.log(i); //  i is not defined
```

## 9.全局块绑定

`let`和`const`不同于`var`在它们的全局范围内的行为。当`var`在全局范围内使用时，它会创建一个新的全局变量。这是全局`window`对象中的一个属性。这意味着我们可能会意外地使用`var`覆盖令人兴奋的全局

```
// in a browservar name = "Hossain";
console.log(window.name); // Hossainvar name = "Rakib";
console.log(window.name); // Rakib
```

它不保存使用`var`变量声明覆盖的表单。但是如果你在全局作用域中使用`let`或`const`，一个新的绑定会在全局作用域中创建，但是它不会在全局`window`对象中添加一个属性。这意味着我们不能使用`let`和`const`覆盖现有的全局

```
// in a browserlet name = "Hossain";
console.log(name); // Hossain
console.log(window.name === name); // falseconst name2 = "Rakib";
console.log(name2); // Rakib
console.log(window.name2 === name2); // false
```

## 10.块绑定的新兴最佳实践

更多的开发者使用 ECMAScript 6。如果不需要改变变量值，我们可以使用`const`，如果不需要改变变量值，我们可以使用`let`。