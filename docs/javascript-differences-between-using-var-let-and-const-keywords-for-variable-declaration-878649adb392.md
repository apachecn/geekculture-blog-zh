# JavaScript:变量声明中使用 var、let 和 const 关键字的区别

> 原文：<https://medium.com/geekculture/javascript-differences-between-using-var-let-and-const-keywords-for-variable-declaration-878649adb392?source=collection_archive---------36----------------------->

`var`关键字是 JavaScript 中用于声明变量的原始关键字。

在 ES2016 中引入，`let`和`const`是用于声明变量的两个新关键字。本文将解释`var`、`let`和`const`关键字工作方式的不同。

在我们进入`let`、`var`和`const`之间的区别之前，理解范围和提升是如何工作的很重要。

![](img/ea5691c3f7c41ce817075111d78065a0.png)

Photo by [Joan Gamell](https://unsplash.com/@gamell?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) / [Unsplash](https://unsplash.com/?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit)

# 范围

作用域是变量的值被定义和访问的空间。

JavaScript 中有 3 种类型的作用域:

*   全球范围
*   功能范围
*   块范围

用`var`关键字定义的变量要么有全局作用域，要么有函数作用域。

用`let`或`const`关键字定义的变量具有块范围。

关于作用域的更深入的解释，请看我的另一篇文章，标题是 JavaScript 中的作用域。

# 提升

当 JavaScript 程序运行时，它将首先解析脚本并寻找任何变量声明或函数声明。如果发现任何变量或函数声明，它将把它们“提升”到各自作用域的顶部，并在继续评估 JavaScript 代码的其余部分之前首先处理它们。这个过程叫做“吊装”

提升影响变量声明，但不影响值初始化/赋值。

JS 中的吊装示例:

记住，提升只适用于变量声明，不适用于变量初始化。下面的例子将返回“undefined ”,因为 x 在第二行被初始化和定义，因此它没有被提升到`console.log()`调用之上。

下面的代码将打印 2。由于变量 y 在第 3 行声明，但没有初始化，它将被提升到程序的顶部，在`y = 2`初始化之上。因此，到实际调用`console.log(y)`时，将为`y`定义值 2。

```
y = 2;
console.log(y); // Returns 2
var y;

// Same As
var y;
y = 2;
console.log(y);
```

**注意:**虽然提升适用于用`var`、`let`或`const`声明的变量，但提升实际上只帮助用`var`声明的变量。用`let`关键字声明的变量如果未初始化就返回`ReferenceError`(更多细节见下面的`TDZ`部分)。你也不能用关键字`const`声明一个变量而不立即初始化它的值。如果您尝试这样做，您将获得一个“syntax error:const 声明中缺少初始化器”。

# var、let 和 const 之间的区别

## 定义变量

使用`var`关键字声明(但未初始化)的变量，如果在初始化之前被访问，将返回一个值`undefined`(参见提升部分)。

```
console.log(x); // Returns undefined
var x = 1; // variable declaration and initialization
console.log(x); // Returns 1
```

用`var`声明的变量可以是函数范围的，也可以是全局范围的。

用`var`声明的变量可以重新声明。

```
var x = 1;
console.log(x); // 1

var x = 2;
console.log(x); // 2
```

## 让

用`let`声明的变量是块范围的。我们只能使用`let`声明相同名称的变量，只要它们在不同的块范围内。

与`var`不同，用`let`关键字声明的变量不能在同一个范围内重新声明

```
let x = 1;
let x = 2; // Uncaught SyntaxError: Identifier 'x' has already been declared
```

然而，您仍然可以重新定义(重新分配)用`let`声明的变量。

```
let x = 1;
console.log(x); // 1

x = 2; // This is ok because you are not trying to redeclare x, just redefine its value
console.log(x); // 2
```

## 时间死区

时间死区(TDZ)是当前作用域中从作用域开始到变量最终初始化之间的区域。TDZ 适用于用`let`关键字声明的变量。用`let`声明的变量不能在 TDZ 内访问(将返回“ReferenceError”)。

## 常数

类似于用`let`声明的变量，用`const`关键字声明的变量是块范围的。

同样类似于`let`，用`const`声明的变量不能重新声明。

然而，与用`let`声明的变量不同，用`const` **声明的变量必须立即用**初始化。否则，您将会以“syntax error:const 声明中缺少初始化器”错误结束。

最重要的是，用关键字`const`声明和初始化的变量不能通过重分配改变它们的值**(见下面的注释)。这是因为`const`关键字导致变量的名称是只读的，从而阻止通过分配的变量对存储在内存中的值进行写访问。仔细想想，这是为什么就说得通了。如果你想创建一个不能轻易改变的变量，你需要知道它的值，否则，你会得到一个带有“未定义”值的常量变量。**

**注意**:注意用`const`关键字初始化的变量不能通过重新赋值改变其值**。这并不意味着常量的值不能改变，只是意味着你不能直接使用变量名来改变它。例如，除了重新分配之外，没有其他方法可以更改字符串或数字变量，但是您可以更改对象的属性。**

# 哪个变量声明是最好的，我应该使用哪个？

我读过韦斯·博斯的一篇文章，我喜欢他的建议:

1.  默认情况下使用`const`关键字声明变量，除非你知道你的变量需要改变它的值(在这种情况下使用`let`)。
2.  如果你知道一个变量的值会改变，使用`let`关键字来声明这个变量(比如迭代器)。
3.  除非特殊情况需要，否则避免使用`var`关键字进行变量声明。

# 摘要:用“var”、“let”和“const”关键字声明的变量之间的差异:

`**var**`

*   范围:全局或函数
*   能够重新申报？是
*   能够重新初始化？是

`**let**`

*   范围:全局或块
*   能够重新申报？不
*   能够重新初始化？是

`**const**`

*   范围:全局或块
*   能够重新申报？不
*   能够重新初始化？不

# 资源

*   [MDN — var](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/var)
*   [MDN — let](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let)
*   [MDN —常量](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/const)
*   [MDN —模块](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/block)
*   [MDN —窗口](https://developer.mozilla.org/en-US/docs/Web/API/Window)
*   [MDN —可变范围](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#variable_scope)
*   [MDN —块语句](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Control_flow_and_error_handling#block_statement)
*   [MDN —吊装](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)
*   [MDN —可变提升](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Grammar_and_types#variable_hoisting)
*   [Var、Let 和 Const——有什么区别？](https://www.freecodecamp.org/news/var-let-and-const-whats-the-difference/)
*   [W3 学校— JavaScript 范围](https://www.w3schools.com/js/js_scope.asp)
*   [雄辩的 JavaScript，编程的现代介绍](https://eloquentjavascript.net/)
*   [JavaScript 变量声明和初始化](https://owlcation.com/stem/JavaScript-Variable-Declaration-and-Initialization)
*   什么是临时死区？
*   [Wes Bos —变量和陈述](https://wesbos.com/javascript/01-the-basics/variables-and-statements/#differences-between-var-let--const)