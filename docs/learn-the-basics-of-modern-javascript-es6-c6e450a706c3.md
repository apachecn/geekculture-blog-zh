# 学习现代 JavaScript 的基础知识(ES6+)

> 原文：<https://medium.com/geekculture/learn-the-basics-of-modern-javascript-es6-c6e450a706c3?source=collection_archive---------15----------------------->

## 你应该在代码中开始使用的 10 个现代特性

![](img/937af9c9256bb1ac66e4316730336b73.png)

您可能已经知道 JavaScript 是一种功能丰富的编程语言，它在每次更新中都得到了增强。

有很多事情要做，你可能仍然会发现自己在用老方法做事。这里有 10 种方法可以让你的 JavaScript 代码现代化。

```
[1\.  Arrow Functions](#393c)[2\.  Default Parameters](#2fe6)[3\.  Object Instead of Switch](#0c89)[4\.  Optional Chaining](#8445)[5\.  Destructuring](#f07b)[6\.  Ternary Operator](#1cd2)[7\.  Nullish Coalescing Operator](#0a66)[8\.  Map and Set](#e345)[9\.  Object.hasOwnProperty()](#537b)[10\. Iterating an Object](#ed8f)
```

## 1.箭头功能

传统函数表达式的简洁替代。有一些[限制](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)，比如没有`this`绑定，但是它仍然可以满足你的一般需求。

下面是传统匿名函数与箭头函数的比较:

已经比较简洁了，但是我们还可以做得更好。当函数体由一行组成时，体括号和`return`关键字可以省略，所以这里我们隐式返回`a + 5`:

```
(a) => a + 5;
```

派对不止于此。当只有一个参数时，我们也可以省略参数括号:

```
a => a + 5;
```

如果我们需要它是一个命名函数呢？我们只是在变量中声明它:

```
const sum = a => a + 5;
```

## 2.默认参数

有时您希望可选参数有一个默认值。这是在函数调用中省略参数时使用的值。

与使用默认参数相比，传统解决方案是这样的:

现在简洁了很多。我们可以对箭头函数做同样的事情:

```
const print = (a = 5) => console.log(`The number is ${a}`);
```

## 3.对象而不是开关

`switch`语句是软件开发的主要部分，因为它可以处理各种场景。不过，有时它可能非常冗长，所以我们可能需要一个更简单的替代方案。

下面是我们如何用对象替换开关:

配对是通过对象`map`完成的。然后，我们使用括号属性访问器来获取相应的值。

## 4.可选链接

当您想要安全地访问一个属性，但仍然保持代码紧凑时，可选的链接操作符`?`非常有用。

让我们看一个没有和有可选链接的例子:

这同样适用于函数调用或数组项访问:

## 5.解构

一个非常方便的表达式，允许您从对象中提取属性或从数组中提取值:

我们使用了括号，然后在括号中指定要提取的属性。这里有一个数组的例子:

这里我们使用方括号从数组中提取第一个和第二个项目。

我们可以将它带到下一个级别，并访问如下所示的嵌套属性:

或者我们甚至可以混合数组和对象析构:

## 6.三元运算符

一种带有三个操作数的运算符:一个条件，如果条件为真则执行的表达式，如果条件为假则执行的表达式。它是对`if else`语句的一种简洁替代。

您还可以通过嵌套条件来获得更复杂的用例:

我建议您谨慎使用这个操作符，因为它可能会变得难以理解。对于更复杂的用例，您可能应该避免它。

## 7.零融合算子

通常你必须为你的变量提供一个默认值。不久前，您唯一简洁的选择是使用逻辑 OR 操作符`||`。不过，你必须小心一件事(现在仍然如此)。如果左操作数为 false，则使用默认值，而不是 false。

有时这可能会造成很大的不同，导致意想不到的行为。让我们看一个例子:

这两个功能对大多数用户来说都很好。然而，对于 id 为`0`的用户，这两个函数的行为会有很大不同:在上面的一个函数中，输出是错误的，因为值为`0`的 id 是 falsy，因此默认为占位符。

然而，底部的一个将正确工作。由于我们已经使用了`??`操作符，它只考虑假左操作数，因此将`0`视为有效值，这就是我们想要的。

这两个操作数都非常有用，你应该根据你要寻找的行为来使用其中的一个。

## 8.地图和布景

低年级学生经常犯的一个错误是对所有事情都使用数组。还有其他数据结构！

当您希望将键与值配对时，Map 类非常方便。它类似于对象，但也有一些[差异](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map#objects_vs._maps)，这可能使地图更适合一些用例。例如，Map 的键可以不同于字符串。

这里有一个例子:

Retrieving a value by key is very easy

另一方面，当您需要存储唯一值的列表时，Set 类非常方便。这意味着，每个值在集合中只出现一次。

Checking for a value existence or deleting it is very easy

与数组相比，改变集合非常容易。当然，Set 类仅用于特定的用例。

## 9.Object.hasOwnProperty()

如果被调用的对象具有指定的属性，则返回`true`的方法。它也相当可靠，因为它只考虑对象自身的属性，而不考虑任何继承的属性。

下面是一个用法示例:

## 10.迭代对象

有时您可能需要遍历对象的属性。当然你不能这么做，因为一个对象是不可迭代的。这里有三种方法可以帮助你:

无论您需要迭代对象的键、值还是两者，每种方法都有不同的结果。

## 包装它

好了，今天就到这里，希望你觉得有用！

感谢阅读。敬请关注更多内容！

## 参考

*   [https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow _ Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)
*   [https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Functions/Default _ parameters](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Default_parameters)
*   [https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Operators/Optional _ chaining](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Optional_chaining)
*   [https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Operators/destructing _ assignment](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)
*   [https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional _ Operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)
*   [https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish _ coalescing _ operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Nullish_coalescing_operator)
*   [https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Global _ Objects/Map](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Map)
*   [https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Global _ Objects/Set](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Set)
*   [https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Global _ Objects/Object/hasOwnProperty](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/hasOwnProperty)
*   [https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Global _ Objects/Object/keys](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/keys)
*   [https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Global _ Objects/Object/values](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/values)
*   [https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Global _ Objects/Object/entries](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries)

## 封面图像

*   照片由[穆罕默德·拉赫马尼](https://unsplash.com/@afgprogrammer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在 [Unsplash](https://unsplash.com/s/photos/developer?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄