# JavaScript:声明和初始化变量，如何在 JavaScript 中存储和访问数据

> 原文：<https://medium.com/geekculture/javascript-declaring-and-initializing-variables-how-data-is-stored-and-accessed-in-javascript-2936f4d69ce0?source=collection_archive---------10----------------------->

在 JavaScript 中声明和初始化变量是两个不同的概念。为了更好地理解这些概念以及变量实际上是什么，让我们从讨论内存如何被用来存储和访问数据开始。

# 什么是变量，它是如何工作的？

变量，也称为“绑定”，将内存中的一段数据连接(或绑定)到一个名称，该名称稍后可用于从内存中检索值。在计算机科学中，内存中的每个位置都有一个内存地址。

声明变量时会发生一些事情:

1.  内存中留出一个位置来存储分配给变量名的未来值(一段数据)。
2.  创建“指向”存储器中该位置的存储器地址。这允许我们在以后访问存储在内存中的值。
3.  变量名与内存地址相关联。

例如，假设您使用 **let** 关键字创建了一个变量 **x** ，并将其值设置为字符串“Hello”。然后使用 **console.log()** 函数将 x 的值记录到控制台。引擎盖下到底发生了什么？

为了更容易解释和理解，我们可以将第一行分成两行代码，一个变量声明和一个赋值。

第 1 行:**设 x；**

*   一个变量被声明为一个名为“x”的变量。
*   内存中的一个位置是为变量“x”的值保留的。
*   指向内存位置的内存地址与变量名“x”相关联。

第二行:x = **“你好”；**

*   JavaScript 查找名为 **x** 的变量，并使用相关的内存地址来访问内存中为变量“x”保留的位置。
*   基本上，变量“指向”存储(或将要存储)值的内存位置。
*   “Hello”的值存储在指定的存储位置。

第 3 行: **console.log(x)**

*   **console.log()** 函数调用变量 **x** 并使用与 **x** 相关联的内存地址来搜索内存中存储的值，该值恰好是“Hello”。
*   特定内存地址的值由 **console.log()** 返回。

简单总结一下，当您试图使用 **console.log(x)** 打印出变量 x 的值时，实际发生的情况是，变量 x 包含存储值“Hello”的内存地址。JavaScript 使用该内存地址转到内存中该内存地址指向的特定位置，并检索值，即“Hello”。

所以**变量“指向”存储在内存中的值。**

# 在 JavaScript 中声明变量

要声明(创建)一个变量，我们需要使用 **var** 、 **let** 或 **const** 关键字，后跟我们想要提供给变量的名称。 **var** 、 **let** 和 **const** 关键字告诉 JavaScript 留出一部分内存，以便我们可以在以后存储特定的数据。

提供给变量的名称可以在以后用于访问内存中分配给变量的位置，并检索存储在其中的数据。要给变量赋值(用值初始化变量)，使用赋值运算符 **=** 将变量名设置为等于一段数据(数字、布尔、字符串、数组、对象、函数等)。)

# 初始化

初始化是一个术语，用于描述将值赋给变量的过程(即，将值(数据段)存储在变量“指向”的内存位置)。

# 资源

*   [MDN — var](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var)
*   [MDN — let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
*   [MDN —常量](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
*   [MDN —区块](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/block)
*   [MDN —窗口](https://developer.mozilla.org/en-US/docs/Web/API/Window)
*   [MDN —可变范围](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#variable_scope)
*   [MDN —块语句](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Control_flow_and_error_handling#block_statement)
*   [MDN —吊装](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)
*   [MDN —可变提升](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#variable_hoisting)
*   Var、Let 和 Const——有什么区别？
*   [W3 学校— JavaScript 范围](https://www.w3schools.com/js/js_scope.asp)
*   [雄辩的 JavaScript，现代编程入门](https://eloquentjavascript.net/)
*   [JavaScript 变量声明和初始化](https://owlcation.com/stem/JavaScript-Variable-Declaration-and-Initialization)
*   [什么是时间死区？](https://www.freecodecamp.org/news/what-is-the-temporal-dead-zone/)
*   [Wes Bos —变量和语句](https://wesbos.com/javascript/01-the-basics/variables-and-statements/#differences-between-var-let--const)
*   [CS50 2020 —第四讲—记忆](https://www.youtube.com/watch?v=NKTfNv2T0FE)