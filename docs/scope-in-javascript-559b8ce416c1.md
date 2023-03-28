# JavaScript 中的范围

> 原文：<https://medium.com/geekculture/scope-in-javascript-559b8ce416c1?source=collection_archive---------19----------------------->

![](img/f5bb04e2beb5638c0c3e9bbb989bd569.png)

Photo by [Arnold Francisca](https://unsplash.com/@clark_fransa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 什么是范围？

作用域是变量(或函数)存在并可访问的代码区域。JavaScript 中有几种不同类型的作用域:全局作用域、函数作用域和块作用域。

# 全球范围

在函数或块之外声明的变量(花括号{ })被认为具有全局范围，这意味着它们可以在 JavaScript 程序中的任何地方被访问

**注意**:当在函数外使用 *var* 关键字定义变量时(因此对其进行全局作用域)，变量被附加到*窗口*对象上。用 *let* 或 *const* 定义的变量不会附加到*窗口*对象上。这是因为*变量*被设计成函数或全局作用域。 *let* 和 *const* 实际上被创建为块范围

*窗口*对象代表浏览器的窗口，包含所有全局函数、对象和变量。

所以从技术上讲，当你在一个 javascript 程序的代码块外使用 *let* 和 *const* 声明一个变量时，它们只在 javascript 程序的全局范围内，而不在*窗口*对象的全局范围内。

# 功能范围

在函数中声明的变量被认为是函数范围的一部分。这个范围被称为*功能范围*。你有时会看到*功能范围*也被称为*局部*范围*范围*。

在函数范围内声明的变量可以从函数内部访问，但不能从函数外部访问。

**注意:**用 *var* 关键字声明的变量在函数内部定义时具有函数(也称为局部)作用域。用*声明的变量 let* 和 *const* 在函数内部声明时，技术上具有块作用域(在花括号内)(即变量的作用域绑定到花括号，而不是函数本身)。因为函数使用花括号来封装函数体，所以函数体的函数作用域和块作用域本质上是一回事。

下面的例子展示了使用*变量*和 *let* 时函数作用域的不同

# 块范围

在 ES2016 中引入了块范围以及 *let* 和 *const* 变量声明关键字。块范围仅适用于用 *let* 或 *const* 关键字创建的变量。

块范围是在一组花括号{ }内定义的范围。花括号定义了代码的“块”，因此被称为“块范围”。

不能在定义块范围变量的块之外访问该变量。

**注意**:用 *var* 关键字 do **NOT** 声明的变量有阻塞作用域，只有用*声明的变量 let* 或 *const* 有阻塞作用域。用 *var* 声明的变量将忽略块范围规则。

请看下面的例子，用 *var* 关键字声明的变量不遵循块作用域:

# 词汇范围

JavaScript 是一种词汇作用域语言(与动态作用域语言相反)。您还会看到词法范围被定义为静态范围。那么词法范围是什么意思呢？

词法作用域意味着作用域是在定义变量和函数的位置定义的(相对于它们运行的位置)。

参见下面关于词法范围如何工作的例子:

我们来分解一下上面的代码片段。

1.当调用 *logVariable( )* 函数时，它会创建一个局部变量 x，并将其值设置为 50。

2.在下一行中，调用了 *myFunc( )* 函数，让我们到定义 *myFunc( )* 函数的地方。

3. *myFunc( )* 在 *x* 变量上调用 *console.log( )* 函数，但是 *x* 没有在 *myFunc( )* 作用域中定义。因此，我们需要上升一个范围到全局范围，以获得值为 1 的 *x* 。(参见下面关于作用域链接的部分)。

请注意，我们从未访问过 *x = 50* 的值，即使它出现在 *logVariable( )* 函数中的 *myFunc( )* 调用的正上方。同样，这是因为词法范围要求我们去定义函数的地方，而不是运行它们的地方。如果 JavaScript 是一种动态作用域语言，调用 *logVariable( )* 将导致控制台记录值 50 而不是 1。

# 范围链接

理解 JavaScript 如何使用作用域链接访问变量是非常有帮助的。理解范围链的概念将有助于您理解如何确定变量/函数的范围。

当一个函数或方法需要访问 JavaScript 中一个没有用当前作用域中的值初始化的变量时，JavaScript 将上升一个作用域级别来查找该变量的值。它将继续向上扩展范围，直到找到函数声明。

**注意:**注意，作用域链接不会向下*到*作用域层次，它只会向上寻找下一个更大作用域中的变量声明

范围链如何工作的示例:

# 参考

[MDN —模块](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/block)

[MDN —变量作用域](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#variable_scope)

[MDN — block 语句](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Control_flow_and_error_handling#block_statement)

[W3 学校 JavaScript 范围](https://www.w3schools.com/js/js_scope.asp)

[W3 Schools JavaScript 窗口—浏览器对象模型](https://www.w3schools.com/js/js_window.asp)

[Wes Bos —范围](https://wesbos.com/javascript/03-the-tricky-bits/scope)

[理解 JavaScript 中的作用域和作用域链](https://blog.bitsrc.io/understanding-scope-and-scope-chain-in-javascript-f6637978cf53)