# 现代 JavaScript 简介

> 原文：<https://medium.com/geekculture/introduction-to-modern-javascript-837ed81f6366?source=collection_archive---------27----------------------->

![](img/4cf80b7311843c7a626ab3a75177cff8.png)

Photo by [Caspar Camille Rubin](https://unsplash.com/@casparrubin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果你是一名软件工程师或 IT 本科生，你可能已经使用过 JavaScript 语言，因为它广泛应用于客户端和服务器端开发。JavaScript 是一种动态的轻量级计算机编程语言。最初，它是由 Netscape 公司在 90 年代初作为 web 开发的脚本语言引入的。众所周知，在 2009 年推出“node.js”之前，JavaScript 在网页开发方面非常出名。在“node.js”的帮助下，我们可以在网络浏览器之外使用 JavaScript。结果，它在后端开发人员中获得了巨大的人气，几年来，JavaScript 一直被认为是世界上使用最多的编程语言。

# ECMAScript

尽管 JavaScript 是开发人员中流行的语言，但由于过去几年没有正确发展，它的名声很差。但是随着 **ECMAScript 6 (ES6)** 的推出，JavaScript 得到了一些重大的改变和更新。那么，这个 ECMAScript 是什么呢？

首字母缩写词 ECMA 代表欧洲计算机制造商协会。ECMAScript 是 JavaScript 和 ActionScript 等脚本语言的规范。ECMA 不时发布标准规范。这些规范为语言带来了新的特性，改善了开发人员的体验。例如，ES5 版本发布了几个新的数组方法，名为“map()”、“reduce()”、“filter()”以及日期函数。说到 ES6 版本，它被认为是迄今为止最大的 ECMAScript 版本之一。它具有 let/const 关键字、类、析构、承诺等特性。

> **注意:** JavaScript 有自己的特性，不属于这些规范的一部分。

在下一章，我将解释 ECMAScript 新版本中引入的一些主要变化。

## 变量范围

JavaScript 有 3 种类型的变量声明关键字，分别命名为， **var，let，**和 **const。**ES6 版本引入了“let”和“const”关键字。这两个关键字在 JavaScript 中提供了**块范围**变量和常量。但是在 ES6 之前，JavaScript 只有两种类型的作用域，命名为**全局作用域**和**函数作用域。**让我们看一个例子来弄清楚。

输出:

```
0
1
2
3
4
Value after iteration: 4
```

正如您在结果中看到的，代码运行没有任何问题。但是如果你用 **"let"** 关键字来声明 **"num "变量**而不是" var "，那么它会在第 6 行得到一个错误。原因是因为“let”提供了块范围。使用“let”关键字声明变量只能在特定的代码块中访问。在这种情况下在 for 循环内。

为了防止上述问题，我们可以在 for-loop 范围之上声明“num”变量，然后在其中使用。当您必须编写包含许多循环和函数的非常长的脚本时，使用 let/const 关键字非常方便。因为您可以在每个代码块中声明同名变量，而不会因为块范围行为而遇到任何问题。

## 常数

从 ES6 版本开始，我们可以在 JavaScript 中使用“const”关键字来声明常量变量。正如我在上一节中提到的，“const”与“let”关键字的行为相同，但有一点不同。因为常量变量不能被重新赋值。我们来看一个例子。

正如你在要点中看到的，我用“const”关键字声明了两个变量。因此，如果您运行前 2 行，它会给您一个错误，而接下来的 2 行(4 & 5)工作没有任何错误。给出错误的原因是显而易见的，因为你不能为常量变量重新赋值。

第 5 行没有给出任何错误的原因，因为 const 只是保护变量，而不是对象或数组。所以即使它是常量，你也可以改变它里面的值，但是仍然不能重新赋值。

## 箭头功能

在 JavaScript 中，有几种定义函数的方法，如下所示。

正如你在要点上看到的，我用 4 种不同的方式定义了 4 个方法。第一个方法声明是 JS 中声明方法的常规方式。所有其他 3 个方法都被声明为在 ES6 中引入的**【箭头函数】**。这些箭头函数允许我们编写比常规方法更短的函数语法。如果您认为常规函数和箭头函数的行为方式相同，请看下面的例子。

输出:

```
This is function1:  { function1: [Function: function1], function2: [Function: function2] }This is function2:  {}
```

正如你在结果中看到的，当你在一个常规函数中调用**“this”关键字**时，它代表了函数的调用者(在这个例子中是“obj”对象)，而 arrow 函数忽略了“this”关键字。

## 对象处理

在 ECMAScript 的新版本中，对象得到了一些合理的改变。所以，让我们看看如何用下面的例子来处理这些问题。

输出:

```
{
 name: 'Nisal',
 age: 25,
 printDetail: [Function: printDetail],
 salary: 100000,
 'project 99': 'Ready for the project'
}
```

“employee”是一个标准对象，在“printDetail()”函数之前，它有常用的键和值对。然后你可以看到一个名为**【薪水】**但是没有值的键。原因是，如果以这种方式定义一个键，就可以从另一个模块中获取值。假设“salary”值依赖于另一个模块，所以可以在对象外部定义一个变量并为其赋值。然后，正如您在输出中看到的那样，将从该值分配“salary”键。

然后您可以看到一个名为**status 的动态属性。**动态属性是一种使用关键字占位符的技术。在第二行代码中，它为该对象中的动态属性分配了一个名为**“project 99”**的键。正如您在输出中看到的，它被替换为指定的键。当你不知道执行时的密钥是什么时，这真的很有用。

## Object .冻结()方法

顾名思义，这种方法用来冻结一个对象。这意味着如果我们对一个对象使用这个方法，我们就不能再改变那个对象了。我们用一个例子来理解这个。

输出:

```
{ name: 'Nisal', contacts: { mobile: '071', home: '034' } }
{ name: 'Nisal', contacts: { mobile: '999', home: '034' } }
```

在上面的要点中，我创建了一个对象，并添加了两个名为 name 和 contacts 的属性。然后我就用了”。freeze()"方法。在那之后，我试图为**“name”**属性分配一个新值，但是正如输出所示，它并没有因为使用而受到影响。freeze()"方法。

但是在第 13 行中，我试图为“联系人”对象的“移动”属性分配一个新值，尽管我已经使用了。freeze()"方法。产生这种结果的原因是，如果您使用"。方法，它将只冻结该对象的第一级值。这意味着它不会影响第二层(内层)对象。所以，如果你要用这个方法，一定要注意这一点。

## 班级

JavaScript 类基本上是创建对象的蓝图。如果你熟悉 Java 或 C#语言，你可能已经知道了类。尽管不完全相同，但仍有一些相似之处。下面的例子让我们看看如何在 JavaScript 中使用类。

输出:

```
The Camera is designed by Canon, and it's Rs:25000
```

如您所见，JavaScript 类中有一些明显的差异。JavaScript 类总是有一个名为**“constructor”**的方法，当一个新对象被创建时，它会自动调用。它用于初始化对象属性，如示例所示。但是方法应该在“构造函数”之外声明

此外，我在示例中使用了继承的概念，用“DSLR”类扩展了“照相机”类。因此，DSLR 类将继承相机类的所有属性和方法。在子类(DSLR)构造函数中，您必须调用 **"super()"** 方法，以便传递父类(Camera)所需的" brand "参数。此外，您可以使用 JavaScript 中的类轻松地覆盖方法。

## 解构

析构是 ES6 版本引入的，它是一种特殊的语法，允许我们将数组或对象解包成一堆变量。当复杂函数有很多参数、默认值等时，析构也很有用。下面的例子让我们看看如何在 JS 中使用析构。

输出:

```
Nisal Pubudu
Nisal Pubudu
Full name: Nisal Pubudu
```

在上面的要点中，我创建了一个名为“student”的对象，然后以常规方式访问这些属性。然后，我使用了对象析构语法，使用 student 对象上相应键的值为三个变量赋值。正如你所看到的，尽管析构语法很简单，但两者都得到相同的输出。

此外，您可以看到有一个名为“fullName()”的函数，它应该打印学生的全名。对于该函数，它只需要“fName”和“lName”字段。在这种情况下，我不需要整个学生对象。因此，我使用了析构语法来实现这一点，如示例所示。

此外，您可以实现数组析构，如下例所示。

```
**const animals = ["Lion", "Parrot", "Shark"];**//array destructuring **const [Mammal, Bird, Fish] = animals;
console.log(`${Mammal}, ${Fish}`);** //Lion, Shark
```

## 承诺

承诺是 JavaScript 中处理异步操作最常见的方式。在承诺之前，开发人员必须使用回调来处理异步操作。因此，他们必须在回调中创建多个回调，最终这种糟糕的做法得到了一个俚语；**《回调地狱》**因此，ES6 推出了轻松处理多个异步操作的承诺。

JavaScript 中承诺的一般行为是:当我们在 JS 中定义一个承诺时，它会在时机到来时得到解决，或者会被拒绝。这基本上是承诺背后的整个想法。

承诺对象有 3 种状态。

*   **完成:**表示承诺的操作成功。
*   **拒绝:**表示承诺的操作不成功。
*   **待定:**承诺仍处于待定状态(尚未履行或拒绝)。

让我们用一个简单的例子来看看如何在 JS 中使用承诺。

输出:

```
Promise is resolved successfully!!!
```

正如你在要点中看到的，我创建了一个名为“myPromise”的承诺，它有两个参数，一个表示成功(解决)，一个表示失败(拒绝)。“condition”变量获取“task()”函数的返回值。为了解释简单起见，“task()”函数总是返回“true”作为值。

然后承诺会检查条件，如果是真的，承诺就解决，否则拒绝。当我们使用承诺时，在承诺被解析后将调用“then()”方法。然后，我们可以决定如何处理已解决的承诺。但是如果承诺失败，那么我们需要使用“catch()”方法。在这个场景中，它满足了承诺的条件，因此承诺将被解决。

所以，这是我的文章的结尾，我希望你喜欢它。快乐编码👨‍💻。

# 参考

[](https://www.tutorialspoint.com/javascript/javascript_overview.htm) [## JavaScript -概述

### JavaScript -概述- JavaScript 是一种动态的计算机编程语言。它是轻量级的，也是最常用的…

www.tutorialspoint.com](https://www.tutorialspoint.com/javascript/javascript_overview.htm) [](https://dev.to/kartik2406/what-is-ecmascript-what-are-its-new-features-31ck) [## 什么是 ECMAScript？它有什么新特点？

### 根据维基百科，“ECMAScript(或 ES)是由 Ecma 标准化的注册脚本语言规范…

开发到](https://dev.to/kartik2406/what-is-ecmascript-what-are-its-new-features-31ck) [](https://www.geeksforgeeks.org/javascript-promises/) [## JavaScript | Promises-GeeksforGeeks

### 承诺用于处理 JavaScript 中的异步操作。在处理多个问题时，它们很容易管理…

www.geeksforgeeks.org](https://www.geeksforgeeks.org/javascript-promises/)