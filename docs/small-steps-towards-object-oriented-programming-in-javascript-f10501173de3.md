# JavaScript 迈向面向对象编程的一小步

> 原文：<https://medium.com/geekculture/small-steps-towards-object-oriented-programming-in-javascript-f10501173de3?source=collection_archive---------18----------------------->

这一周充满了阻碍我编码之旅的“生活”。但这并不意味着我放松了督促自己去学习和练习。这只是意味着我必须记住我的冒名顶替综合症的策略和大量的时间管理。

因此，这篇文章简短扼要，回顾了 JavaScript 中与面向对象编程相关的一些概念，以及每个人都喜欢的“新箭头”语法(又名胖箭头)。一如既往，更多的链接到外部资源的深度潜水包括在内！

![](img/620a228b9d09f13f029e426c834d6b82.png)

Image from Pixabay by: [Graphic Mama- Team](https://pixabay.com/users/graphicmama-team-2641041/)

现在为什么要学习面向对象编程？

本周是面向对象编程的速成班。所以，我一直致力于用 JavaScript 创建类，这不同于 HTML 类。但是我们在 CSS 中设计 HTML 类时使用的继承思想是相似的。JavaScript 中的类与 HTML 中的类遵循相似的家族树结构。JavaScript 类帮助我们创建对象，保持它们有组织，并编写更有效的代码，尤其是当我们试图枯竭它的时候！

一旦面向对象编程、闭包和重构变得更像是第二天性，那么我会对学习 React 更有信心，但那仍然是一条路要走。还有更多 JavaScript 要先学！

**新箭头的用例= >函数语法**

新的箭头(有时称为粗箭头)函数语法很有帮助，因为它允许我们编写更短的函数。

BEFORE: hello = function(){ return“朋友们好！”};

AFTER: hello = () => {return“朋友们好！”};

但是更值得注意的是，新的 arrow 函数语法以不同的方式处理“this”。对于箭头函数，没有“这个”的绑定。

在常规函数中，“this”关键字表示调用函数的对象(例如，文档、按钮等)。).但是使用箭头函数时，“this”关键字总是表示定义箭头函数的对象。

以上解释基于 [W3 学校](https://www.w3schools.com/js/js_arrow_function.asp)

**经典面试问题:**解释 function foo() {}和 const foo = function() {}在 foo 用法上的区别

a)函数 foo(){}

B) Const foo = function (){}

答:函数语句被吊到最上面。

b:使用 const 我们可以声明一个变量(foo ), const 给它一个块作用域，这样就停止了对顶部的完全提升，并确保它不能被重新声明。

**在构造函数中对方法使用箭头语法有什么好处？**

对于面向对象的编程，箭头语法是至关重要的。对于箭头函数，新函数不定义自己的“this”值。使用箭头函数，“this”被绑定到新创建的对象上，因此在面向对象编程中会产生更干净的代码和更好的可维护性。

什么是析构对象或数组？

JavaScript 中的析构允许我们从数组、对象、映射和集合中提取数据到它们自己的变量中。它允许我们从一个对象中提取属性，或者从一个数组中提取项目(或者一次提取多个)。我们不是创建多个变量，而是将它们析构到一行代码中。

示例:

Const user = {first: 'Rachel '，last: 'Smith '，county: 'Travis '，city:'奥斯汀' }；

Const {first，last } = user

文档和示例:【https://wesbos.com/destructuring-objects 

**什么是闭包，我们如何使用它？**

闭包允许从内部函数访问外部函数的作用域。在 JavaScript 中，闭包是在每次创建函数时创建的。这意味着当我们在 node.js 中进行单元测试时，如果我们正确地写出测试，闭包会有所帮助。因为本质上，闭包是一个被赋值的函数(或者在单元测试作为一个值返回的情况下)及其相关的作用域。

JavaScript 中关于闭包的有用资源:

[https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Closures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Closures)

[https://medium . com/JavaScript-scene/master-the-JavaScript-interview-what-a-closure-b2f0d 2152 b 36 #:~:text = To % 20 use % 20a % 20 closure % 2C % 20 define，the % 20 outer % 20 function % 20 has % 20 returned](/javascript-scene/master-the-javascript-interview-what-is-a-closure-b2f0d2152b36#:~:text=To%20use%20a%20closure%2C%20define,the%20outer%20function%20has%20returned)。

[](https://blog.bitsrc.io/closures-in-javascript-why-do-we-need-them-2097f5317daf) [## JavaScript 中的闭包:我们为什么需要它们？

### 了解如何回答一个最常见的 JavaScript 面试问题。

blog.bitsrc.io](https://blog.bitsrc.io/closures-in-javascript-why-do-we-need-them-2097f5317daf) 

我希望这能帮助任何正在学习面向对象编程基础的人，或者正在为那些可怕的面试问题而学习的人！

你的代码朋友，

雷切尔(女子名)