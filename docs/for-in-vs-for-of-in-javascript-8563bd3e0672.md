# for…in vs for…of in JavaScript

> 原文：<https://medium.com/geekculture/for-in-vs-for-of-in-javascript-8563bd3e0672?source=collection_archive---------24----------------------->

![](img/87cb3257e04d2aa027b9b75172d5fb56.png)

有相当一段时间，我一直在努力完全理解`for…in`和`for…of`的区别。如果您通过 Google 或 dev.to feed 找到了这个，我可以有把握地假设您可能也在想同样的事情。`for…in`和`for…of`是我们都熟悉的`for`回路的替代。然而，`for…in`和`for…of`用在不同的场合取决于你在找什么，而我们所知道的`for`循环基本上可以用在任何场合。

我们将首先检查例子/用法，然后我们将分解它们。

## 例子/用法

**👉🏻** `**for**`

```
const arr = [1, 2, 3, 4, 5]function printArr(arr) {
 for (let i = 0; i < arr.length; i++) {
 console.log(arr[i]);
 }
}console.log(printArr(arr));
// 1
// 2
// 3
// 4
// 5
```

**👉🏻** `**for…in**`

```
const obj = { a: 1, b: 2, c: 3 }
function printObj(obj) {
 for (let prop in obj) {
 console.log(`prop: ${prop}`)
 console.log(`obj[prop]: ${obj[prop]}`)
 }
}console.log(printObj(obj));// prop: a
// obj[prop]: 1
// prop: b
// obj[prop]: 2
// prop: c
// obj[prop]: 3
```
```

**👉🏻** `**for…of**`

```
const arrOf = [1, 2, 3, 4, 5]
function printArrOf(arr) {
 for (let ele of arr) {
 console.log(ele);
 }
}console.log(printArrOf(arrOf));// 1
// 2
// 3
// 4
// 5
```

现在你看到了它们是如何被使用的，让我们一个一个地分解它们吧！

## 我们亲爱的好朋友,“for”声明

这个基本的`for`循环可以在我们需要迭代的任何时候使用。

**基本语法**

```
for ([initialization]; [condition]; [final-expression]) {
 statement
}
```

迭代通常发生在`block`(又名`{}`)内部。我们会将多个语句放在要执行的循环块中。您可以使用`break`、`continue`等。根据情况继续或中断`for`循环。

**示例同** `**break**`

```
for (let i = 0;; i++) {
 console.log(i);
 if (i > 5) break;
}// Expected Output: 
// 0
// 1
// 2
// 3
// 4
// 5
// 6// Explanation: The loop breaks when i is larger than 5.
```

✨快速提示:括号内的所有表达式都是可选的。

**示例同** `**continue**`

```
for (let i = 0; i < 10; i++) {
 if (i === 7) continue;
 else console.log(i);
}// Expected Output:
// 0
// 1
// 2
// 3
// 4
// 5
// 6
// 8
// 9// Explanation: if i is equal to 7, we will skip that i and move on to the next index.
```

## “为了…在”,主角#1

`for…in`循环遍历一个对象的所有**可枚举属性**。

如果你不知道什么是 enumerable，我会尽力解释它是什么。基本上你可以认为可枚举属性是对象中键值对的`key`。它也会在`Object.keys()`方法中出现。因此，如果我们看一下上一节的例子…

```
const obj = { a: 1, b: 2, c: 3 }function printObj(obj) {
 for (let prop in obj) {
 console.log(`prop: ${prop}`)
 console.log(`obj[prop]: ${obj[prop]}`)
 }
}console.log(printObj(obj));// prop: a
// obj[prop]: 1
// prop: b
// obj[prop]: 2
// prop: c
// obj[prop]: 3
```

`prop`是键值对中的`key`,这是我们的可枚举属性。如果你对如何检索一个对象的值有基本的了解，我们把键当作数组中的索引，并把它放在方括号中👉🏻`obj[prop]`，像这样。

```
const obj = { 
 name: “Megan”, 
 age: “do the Math”, 
 role: “front-end developer” 
}for (const property in obj) {
 console.log(property);
}// Expected Output:
// name
// age
// role
```

到目前为止，我们的例子都是在 object 或`{}`(因为 array 也是一个对象)中，使用`for…in`迭代一个数组是不推荐的/好的做法，因为索引顺序很重要。

是的，数组索引也是可枚举的属性，但是是整数。如果我们使用`for…in`来迭代一个数组，它的行为很不可预测。不能保证元素以特定的顺序迭代。此外，如果要通过分配超出数组当前大小的索引来扩展数组，它可能不会反映正确的索引。因此，带有数字索引的`for…of`、`forEach`或`for`循环是迭代数组的更好方法。看看下面这篇文章中展示的例子👇🏻

进一步阅读:

*   [Johannes Baum 的 JavaScript](https://betterprogramming.pub/3-reasons-why-you-shouldnt-use-for-in-array-iterations-in-javascript-8db3f42d8c73) 中不应使用“for…in”数组迭代的 3 个理由

## “给…的”,主角#2

这是我们的第二个主角`for…of`。以防你不知道，`for…of`在 ES6 中有介绍。`for…of`已经成为很多 JavaScript 开发人员有用的迭代方法。`for…of`可以迭代任何**可迭代对象**。你说吧，`String`，`Array`，`Object` …

**字符串**

```
const name = “Megan”;for (const alphabet of name) {
 console.log(alphabet);
}// Expected Output:
// M
// e
// g
// a
// n 
```

**数组**(从示例部分复制)

```
const arrOf = [1, 2, 3, 4, 5]function printArrOf(arr) {
 for (let ele of arr) {
 console.log(ele);
 }
}// Expected Output:
// 1
// 2
// 3
// 4
// 5
```

**对象**(在`Object.entries()`的帮助下)

```
const obj = {
 name: “Megan”, 
 age: “do the Math”, 
 role: “front-end developer” 
};for (const [key, value] of Object.entries(obj)) {
 console.log(key, value);
 console.log(`${key}: ${value}`);
}// Expected Output:
// name Megan
// name: Megan
// age do the Math
// age: do the Math
// role front-end developer
// role: front-end developer// Explanation: the [key, value] is a destructure of the result from Object.entries.
```

**🐧侧边栏注释🐧**
`Object.entries()`返回给定对象自身的可枚举字符串键属性的数组。

```
const obj = {
 name: “Megan”, 
 age: “do the Math”, 
 role: “front-end developer” 
};Object.entries(obj)
// [
// [ ‘name’, ‘Megan’ ],
// [ ‘age’, ‘do the Math’ ],
// [ ‘role’, ‘front-end developer’ ]
// ]
```

进一步阅读:

*   [揭开 ES6‘for-of’循环的神秘面纱](https://danielmjung.medium.com/demystifying-the-es6-for-of-loop-9c1a0166d1c6)作者 Daniel Jung
*   [为什么 JavaScript 中 for…of 循环是宝石](https://dmitripavlutin.com/javascript-for-of/)

## 什么时候应该用哪一个？😯

本节的目的是将这两个`for`语句“并排”放在一起，这样我们就可以进行比较。

这里有一个简单的方法来记住这一点:
⭐️在迭代对象的可枚举字符串键属性对时使用`for…in`。你知道`{ blah1: blah blah, blah2: blah blah blah }`。但不是`ARRAY`！！记住，无论记录的是什么，都会像记录数组的索引一样，但是是字符串，所以如果你想记录/返回值，一定要用`obj[key]`打印出来。
⭐️在迭代可迭代对象时使用`for…of`:`String`，`Array`等。

进一步阅读:

*   [MDN 在](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/for...of#difference_between_for...of_and_for...in)中 for…of 和 for…之间的差异

下次当你在做一些需要迭代的东西，或者只是做你常规的 Leetcode 练习，或者更好…在你的技术面试中，用`for…of`和`for…in`展示你新获得的知识。

最后但同样重要的是…编码快乐！

![](img/19d39945d3cc7ef88bbba66ebafdfd79.png)

我的媒体读者们，你们好！这篇文章最初发布在 [dev.to](https://dev.to/mehmehmehlol/for-in-vs-for-of-in-javascript-174g) (文章的直接链接)上，因为我目前在那个平台上更活跃。我在这里张贴同样的内容，因为我想接触到更多的观众。如果您喜欢您正在阅读的内容，请关注我的[开发到](https://dev.to/mehmehmehlol)(我的个人资料)！