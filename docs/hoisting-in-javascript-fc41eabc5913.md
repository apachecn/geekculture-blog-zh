# 在 JavaScript 中提升

> 原文：<https://medium.com/geekculture/hoisting-in-javascript-fc41eabc5913?source=collection_archive---------18----------------------->

让我们谈谈 JavaScript 中的提升，它是如何工作的，以及我们如何从中受益。

![](img/bcd9ccee120889fc36f38822bf789325.png)

Photo by [Andrey Kremkov](https://unsplash.com/@spinaldog?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

[MDN Web Docs 给出了吊装的如下定义](https://developer.mozilla.org/en-US/docs/Glossary/Hoisting)

> 例如，从概念上讲，提升的严格定义表明变量和函数声明被物理地移到了代码的顶部，但事实上并不是这样。相反，变量和函数声明在编译阶段被放入内存中，但停留在您在代码中键入它们的位置。”

无论变量和函数声明在代码中的什么位置定义，它们都在运行时或代码执行前被放入内存。

**变量吊装**

好了，这个写得很好的定义表明我们需要处理变量和函数声明，让我们先看看变量。为了定义一个变量名，我们可以考虑两个基本的选项来编写我们的代码，第一个也是最常见的，我们声明一个变量，然后使用赋值操作符(=)来给这个变量赋值。

```
var someGreeting = "Hi Bob!";
// declaration & assignment or initializationsomeGreeting; // = > Hi Bob!
// referencing to a variable
```

根据具体情况，我们可以现在声明一个变量，以后再给这个变量赋值。

```
var someGreeting;
// declarationsomeGreeting = "Hi Bob!";
// assignmentsomeGreeting; // = > Hi Bob!
// referencing to a variable
```

正如上面演示的那样，在初始化变量后引用它是一个好的做法；在这两种情况下， *someGreeting* 变量返回一个赋值/字符串“嗨，鲍勃！”

如果我们在初始化之前定义一个变量会发生什么？让我们在代码示例中将变量名 *someGreeting* 上移，放在初始化之前。

```
console.log(someGreeting); // = > undefined
// referencing to a variablevar someGreeting = "Hi Bob!";
// declaration & assignment or initialization
```

和

```
console.log(someGreeting); // = > undefined
// referencing to a variablevar someGreeting;
// declarationsomeGreeting = "Hi Bob!"; // = > "Hi Bob!"
// assignment
```

如这些例子所示，在初始化之前引用变量会导致返回“未定义”。未定义的属性表示变量没有赋值，或者根本没有声明。请记住这句话，我们以后会用到它。

如果我们引用一个在代码中不存在的不同变量，会发生什么？让我们引入一个名为 *sayBye* 的全新变量，它在我们的代码中从未使用过。

```
console.log(someGreeting); // = > undefined
// referencing to a variablevar someGreeting = "Hi Bob!";
// declaration & assignment or initializationsomeGreeting; // = > Hi Bob!
// referencing to a variablesayBye; // => ReferenceError: sayBye is not defined
```

和

```
console.log(someGreeting); // = > undefined
// referencing to a variablevar someGreeting;
// declarationsomeGreeting = "Hi Bob!"; // = > "Hi Bob!"
// assignmentsayBye; // => ReferenceError: sayBye is not defined
```

嗯嗯……很有趣，不是吗？JavaScript 抛出 *ReferenceError: sayBye 未定义*(reference error 对象表示引用不存在的变量时出错)。

众所周知，JavaScript 引擎是从左到右、从上到下读取代码的，如果是这样，那么为什么下面的代码会导致“未定义”而不是抛出这个 *ReferenceError* ？为什么我们会有如此不同的行为？有什么区别？

```
1 console.log(someGreeting); // = > undefined
2 // referencing to a variable
3
4 var someGreeting = "Hi Bob!";
5 // declaration & assignment or initialization
```

这都叫吊装！

让我们来了解一下这种吊装是如何进行的。我们已经声明了一个 *var someGreeting = "Hi Bob！*”；现在让我们在运行时一行一行地检查我们的代码。首先会发生什么？在运行第一行代码(第 1 行)之前，JS engine 识别代码中的所有变量(在我们的例子中是第 4 行),并将它们的名称存储在内存中，每个名称都有一个缺省值 undefined。JS 机器找到变量 *someGreeting* (第 4 行)，在内存中创建它的名字，并给这个名字附加一个 undefined 值，意思是 *someGreeting = undefined* ，这就是我们在执行第一行代码之前在内存中的内容。

这就是为什么在初始化之前定义的变量 *someGreeting* 返回“undefined”，而 *sayBye* 抛出这个 *ReferenceError 消息*。这个 *sayBye* 在我们的代码中不存在，所以在代码执行之前，JavaScript 内存中不会存储任何东西。回到我们的陈述“一个未定义的属性表示一个变量没有被赋值，或者根本没有被声明”这也是正确的，因为我们赋值了一个值*“嗨 Bob！”*到 *someGreeting* 变量只在第 4 行。

**功能提升**

定义函数的常用方法有哪些？函数声明和函数表达式。

函数声明包含函数名、返回关键字、参数和代码块。当一个函数被声明后，它可以在任何时候被调用。

函数表达式与函数声明非常相似，语法几乎相同，它有一个 return 关键字、参数和一段代码，唯一的区别是它没有函数名。因为它没有这个名字，所以它可以被称为匿名函数，它的结果可以存储在一个变量名中，这样我们就可以调用这个函数。

这是对我们两个函数的正确和常见的调用，我们已经定义了函数声明或函数表达式，只有这样，我们才调用这些函数。没有区别，任何时候我们想要调用命名函数，我们使用下面演示的语法。

```
// function declaration
function funDeclaration (a, b) {
return a + b;
};// function expression
var funExpression = function (a, b) {
return a + b;
};funDeclaration (5, 10); // => 15
// invocationfunExpression (15, 20); // => 35
// invocation
```

如果我们像对待变量一样，在创建变量之前调用这些函数，会发生什么？让我们将函数声明的调用移到函数声明之上，并运行下面的代码。

```
1 funDeclaration (5, 10); // => 15
2 // invocation
3
4 // function declaration
5 function funDeclaration (a, b) {
6   return a + b;
7 };
```

代码的结果是“15”，因为 JavaScript 中的函数声明被提升到封闭函数或全局范围的顶部。为什么会这样，背后的逻辑是什么？让我们在运行时一行一行地检查代码。如前所述，在运行第一行代码(第 1 行)之前，JavaScript 引擎会识别代码中的所有变量，并将它们的名称存储在内存中，每个名称的默认值为 undefined。其次，在执行第一行代码之前，JavaScript 引擎识别代码中的所有函数声明，并将它们移入内存。函数体出现并被初始化以供立即使用。也就是说，我们不需要等到 JavaScript 引擎到达第 5 行来初始化函数，数据已经可用，函数也可以动态初始化，所以我们不需要按照先声明函数再调用函数的方式来构建代码，因为提升可以反过来进行。

函数表达式呢？在下面的示例中，函数表达式的调用被上移至函数表达式之上。

```
funExpression (15, 20); //TypeError: funExpression is not a function
// invocation
// function expression
var funExpression = function (a, b) {
return a + b;
};
```

这段代码导致 *TypeError: funExpression 不是函数*，因为 JavaScript 中的函数表达式不像函数声明那样被提升。上述函数声明的逻辑不适用于函数表达式，因此在创建函数表达式之前不能使用它们。变量名呢？变量名不是像我们之前探索的那样被吊起来了吗？让我们运行下面的代码。

```
console.log(funExpression); // =>  undefined
// function expression
var funExpression = function (a, b) {
return a + b;
};
```

即使变量名被提升了，定义(函数体)却没有，所以这段代码返回“undefined”。

**箭头功能**

一旦我们了解了函数声明和函数表达式，以及它们之间的区别，将这些知识应用于 JavaScript 中的其他类型的函数就变得相对容易了，例如，箭头函数。箭头函数只是传统函数表达式的一种简短而紧凑的替代形式。提醒一下，表达式是解析为值的任何有效代码，也就是说，我们可以说箭头函数实际上是一个箭头函数表达式，它的操作类似于函数表达式，所以就像函数表达式一样，箭头函数表达式是不提升的。

这是一个箭头函数的例子。当使用箭头函数时，我们首先必须在调用函数之前定义一个函数。这是声明和调用箭头函数表达式的正确方法。

```
// arrow function expression
var arrowFuncExpression = () => {
console.log("This is an arrow function expression");
};arrowFuncExpression(); // => This is an arrow function expression
// invocation
```

让我们在代码中向上调用这个箭头函数表达式，并运行代码。

```
// invocation
arrowFuncExpression(); // => TypeError: arrowFuncExpression is not a function// arrow function expression
var arrowFuncExpression = () => {
console.log("This is an arrow function expression");
};
```

这段代码导致*type error:arrow func expression 不是一个函数*，和我们之前观察到的函数表达式一样，箭头函数没有被提升。

![](img/1e28607e1f51318c4d92d3150e3ac306.png)

Photo by [Tudor Baciu](https://unsplash.com/@baciutudor?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**总结:**

*   变量名被提升到作用域的顶部，并返回值 undefined。
*   函数表达式的变量名被提升到作用域的顶部，并返回值 undefined。
*   函数声明被提升到作用域的顶部，与它们的主体一起，您可以在声明它们之前调用函数声明。
*   函数表达式不被提升，即使变量名被提升，定义(函数表达式体)不被提升，因此函数表达式应该在它们的声明/定义之后被调用。
*   例如，像箭头函数和/或箭头函数表达式这样的函数表达式的导数不被提升。

如果你觉得这些信息有用，请随时关注我。希望你喜欢这篇关于 JavaScript 提升的简短指导，请保持关注！