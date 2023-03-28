# 编写干净的 JavaScript

> 原文：<https://medium.com/geekculture/writing-clean-javascript-es6-edition-834e83abc746?source=collection_archive---------0----------------------->

![](img/b9e2875ad6fce72b1b6d574fd8b66636.png)

Photo by [Redaco](https://dribbble.com/redaco) on [dribbble](https://dribbble.com)

干净的代码不仅仅是有效的代码，而是可以被其他人容易地阅读、重用和重构的代码。编写干净的代码很重要，因为在典型的工作环境中，您不是为自己或机器编写代码。实际上，你是在为一群需要理解、编辑和构建你的作品的开发人员写作。

本文关注于编写干净的 **JavaScript** 代码，这些代码*不是特定于框架的*，尽管如此，大多数提到的例子可以适用于几乎任何其他编程语言。一般来说，以下概念大多是从 Robert C. Martin 的书 [*Clean Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) ，中采纳的建议，并不意味着要严格遵循。

# 1.变量

## 使用有意义的名称

变量名应该是描述性的。经验法则是大多数 JavaScript 变量都是 Camel Case ( *camelCase* )。

请注意，布尔名称通常回答特定的问题，例如:

```
isActive
didSubscribe
hasLinkedAccount
```

## 避免添加不必要的上下文

当上下文已经由包含的对象或类提供时，不要向变量名添加多余的上下文。

## 避免硬编码值

不要插入常量值，确保声明有意义和可搜索的常量。注意，全局常量可以在尖叫蛇案例(*尖叫 _ 蛇 _ 案例*)中风格化。

# 2.功能

## 使用描述性名称

函数名可以很长，只要它们描述了函数实际做的事情。函数名通常采用动作动词的形式，返回布尔值的函数可能是个例外，它可以采用“*是或否*问题的形式。函数名也应该是驼色的。

## 使用默认参数

默认参数比在函数体中使用短路或额外的条件语句更简洁。尽管如此，在这里重要的是要记住，短路对所有被认为是“*falsy*”*的值都有效，例如`false`、`null`、`undefined`、`''`、`""`、`0`和`NaN`，而默认参数只替换`undefined`。*

## *限制参数的数量*

*尽管这条规则可能有争议，但函数应该有 0、1 或 2 个参数。有三个参数已经太多了，超出这个范围意味着两种情况中的任何一种:*

*   *职能已经在做很多了，应该分。*
*   *传递给函数的数据是有某种联系的，可以作为专用的数据结构传递。*

## *避免在一个函数中执行多个操作*

*一个函数应该一次做一件事。这条规则有助于减少函数的大小和复杂性，从而使测试、调试和重构更加容易。函数中的行数是一个强有力的指标，应该对函数是否执行了许多操作给出一个标志。一般来说，尽量少于 20-30 行代码。*

## *避免使用标志作为参数*

*一个参数中的标志实际上意味着该函数仍然可以被简化。*

## *不要重复自己(干)*

*重复的代码从来都不是好现象。如果你重复你自己，每当逻辑有变化时，你将不得不更新多个地方。*

## *避免副作用*

*在 JavaScript 中，你应该更喜欢函数模式而不是命令模式。换句话说，除非需要，否则保持函数的纯洁性。副作用会修改共享的状态和资源，导致不希望的行为。所有副作用都应该集中；如果您需要改变一个全局值或者修改一个文件，那么只需要一个服务。*

*此外，如果将一个可变值传递给一个函数，您应该返回该值的一个新的变异克隆，而不是直接变异该值并返回它。*

# *3.条件式*

## *使用非否定条件句*

## *尽可能使用短工*

## *避免分支并尽快返回*

*提前返回将使你的代码更线性，更易读，更简单。*

## *优先使用对象文字或映射，而不是 switch 语句*

*每当这种情况发生时，使用对象或映射进行索引将减少代码并提高性能。*

## *使用可选链接*

# *4.并发*

## *避免回访*

*[回调](https://developer.mozilla.org/en-US/docs/Glossary/Callback_function)很混乱，导致嵌套代码。 [ES6](https://www.w3schools.com/js/js_es6.asp) 提供[承诺](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)允许链接回调，从而产生更干净的代码。然而， [ECMAScript 2017](https://262.ecma-international.org/8.0/) 还提供了“ [Async/Await](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function) ”语法，作为一种可以说是更干净的解决方案，它为代码施加了进一步的线性。*

# *5.错误处理*

## *处理抛出的错误和拒绝的承诺*

*无需提及为什么这是一条极其重要的规则。现在花时间正确地处理错误将减少以后不得不寻找错误的可能性，特别是当您的代码进入生产阶段时。*

# *6.评论*

## *仅注释业务逻辑*

*可读的代码可以让你避免过度注释你的代码。因此，你应该只评论复杂的逻辑。*

## *利用版本控制*

*绝对没有理由保留注释代码或日志注释，版本控制已经在那里处理了。*

## *尽可能记录*

*文档有助于提高代码质量和可靠性。它可以作为你的代码库的用户手册，任何人都可以理解你代码的所有方面。*

```
*/**  
 * Returns x raised to the n-th power.  
 *  
 * @param {number} x The number to raise.  
 * @param {number} n The power, should be a natural number.  
 * @return {number} x raised to the n-th power.  
 */ 
function pow(x, n) {   
    // ...
}*
```

*本文简要讨论了使用最新语法增加 JavaScript 代码的一些重要步骤。同样，这些概念中的大部分都可以推广并应用于不同的编程语言。采用这些实践可能需要一些时间，特别是对于较大的代码库，但是从长远来看，将保证您的代码变得可读、可伸缩和易于重构。*

*[![](img/169f8b1142e42f66cc1f8d36823ed21c.png)](http://buymeacoffee.com/jalkhurfan)

Support me to write more!*