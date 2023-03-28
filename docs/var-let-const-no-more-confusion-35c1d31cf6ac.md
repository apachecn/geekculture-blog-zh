# Var，Let，Const——不再混淆

> 原文：<https://medium.com/geekculture/var-let-const-no-more-confusion-35c1d31cf6ac?source=collection_archive---------56----------------------->

![](img/917e9a2c5696d5ab7a2c78626255d91c.png)

有那么多程序员自称热爱 JavaScript，用 JavaScript 编码，吃 JavaScript，睡 JavaScript。但是没有一个程序员不需要经历一段艰难的时间来理解它的许多模糊的特性和它们不可预测的行为。

如果对作用域和变量的生命周期缺乏正确的理解，JavaScript 变量声明的模糊性(函数和对象也可以存储在变量中)会让程序员的生活变得一团糟。

您可能知道，在 JavaScript 中，var、let 和 const 用于声明变量。提醒你一下，javascript 中还有另外一种声明，完全不需要关键字。我们称这些变量为“全局变量”。当我们调用全局变量时，它应该在代码中的任何地方都是可访问的。

还有一些我们不希望表现得像全球变量一样的变量。我们希望它们被限制在特定的代码块中。我们称这种变量为“局部变量”。当我们说“某个代码块”这样的话，就会引出另一个核心 JavaScript 特性“范围”。

但是，在理解变量声明以及 var、let 和 const 之间的区别时，我们与作用域和全局/局部变量到底有什么关系呢？暂时不回答这个问题，我们现在将进入下一部分。我保证在这篇博客结束时，你自己能够回答这个问题。

# 让我们首先处理范围

像许多其他编程语言一样，作用域是 javascript 的一个重要特性，通常指某个特定的代码块。作用域中声明的任何东西都只能从它自己的世界中访问。在它之外，那些变量是没有意义的。例如，如果我们在函数中声明任何东西，我们就不能在函数外部访问它。据说它们是其边界内的本地。javascript 中主要有两种类型的作用域。

1.  全球。
2.  本地的。

在 javascript 中，任何没有任何特定关键字声明的变量都具有全局范围。例如，“窗口”是浏览器中的一个全局对象。

为了便于理解，我们可以将局部范围进一步划分为两个独立的范围。

1.  功能范围。
2.  阻止范围。

函数内部的一切都被称为有函数作用域。这意味着当函数停止被调用时，它的所有变量也会消失。

```
function Add(a,b){
var sum = a+b; // sum is a local variable in this function;
return sum;
 }console.log(Add(2,3)) // returns 5 console.log(sum) // throws reference error. as there is no sum outside the function.
```

但是如果我们写这样的东西呢

```
function Add(a,b){ 
sum = a+b;
return sum 
}
console.log(Add(2,3)); // output : 5 
console.log(sum) ; // output : 5
```

我们已经看到，尽管我们没有在函数之外的任何地方声明“sum ”,我们仍然可以访问和打印它。函数内部声明的东西不应该是“局部”变量吗？

坚持住！我们这里不这么做。游戏才刚刚开始！

还有一个作用域叫做块作用域。函数作用域也是一种块作用域，但不是每个块作用域都是函数作用域。任何循环，if/else 条件块和开关都称为块作用域。即使你把任何东西放在一对花括号里面，它也会有一个 block 作用域。

```
{

let a = 5; 
const b = 10;
console.log(a) // returns 5 
console.log(b) // returns 10 } 
console.log(a) // Reference error, a is not defined.
console.log(b) // Reference error , b is not defined.
```

我们已经看到，a 和 b 只能在块范围内访问。到目前为止，一切顺利！给你一个提示，如果我们这样写-

```
{
var a = 5;
console.log(a)// returns 5 
} 
console.log(a) // returns 5
```

我们在块中声明了变量“a ”,它应该被限制在它自己的世界中。那么，即使我们没有在块外声明 a，我们又如何能够打印它呢？

如果你仔细看看前面的两个代码片段，你会注意到，我们在第一个例子中用“let”声明了一个”,在第二个例子中用“var”声明了它。

现在你可能已经猜到了，在作用域和带有 var、let 和 const 的声明之间一定有什么需要关心的。从现在开始，我们将开始看到真正的图片。你准备好了吗？

# 全局变量

当我们声明任何没有像 var/let/const 这样的关键字的变量时，它被认为具有全局范围，可以从代码中的任何地方访问。因此，即使您在函数或循环中声明这样的全局变量，它们也不会绑定到自己的块中。它们只是属于全局窗口(在浏览器的情况下)对象。现在回到上面的例子，我们希望全局变量“sum”被限制在它的函数范围内，并再次思考为什么它没有发生。

# 风险值的世界

“var”严格限定在声明它的函数范围内。我重复“功能”这个词。是的，var 是函数范围的！所以我们用 var 声明的任何东西，应该只能从函数内部访问，在函数外部没有任何意义。这意味着，我们仍然可以访问块范围之外的 var 类型变量，比如“loop”/if-else/或者一对 palin 花括号。这就是为什么我们能够在前面的一个例子中在块外打印这样的变量。让我们再看一遍

```
{ 
var a = 5;
 } 
console.log(a) // returns 5;
```

在这个例子中，尽管我们没有在块外的任何地方声明变量“a ”,我们仍然可以打印它。记住规则！用 var 声明的任何东西都是函数范围的。在示例中，花括号内的块不同于函数的块。但是等等！也没有明确的函数。实际上，javascript 中的一切都被认为是在一个名为 window 的全局对象的范围内运行[在 Nodejs 中它被称为“Global”]。当变量“a”知道在它被定义的地方没有显式函数时，它就像它的属性一样被附加到窗口对象上，并开始表现得像一个全局变量。这并不一定是因为任何用“var”声明的变量都应该具有全局范围。初学者常见的困惑。你明白为什么了！

# 出租和出租王国

任何用 let 和 const 声明的变量都有块作用域。这里所说的 block 还包括 function 以及 loop、conditional 和 block with 花括号。如果我们试图访问任何 let，const 类型变量，我们不能这样做。

```
{ 
let name ="reza";
let age = 25; 
const nationality = "Bangladeshi" 
} 
we can't access those variables here.
```

实际上，与“var”相比，let 和 const 给了我们更多的可预测性和一致性。下面提到了一些。

```
{ 
let x = 5; 
let x =7; // it will raise syntax error , as x can't be redeclared within same scope. 
} var name = "reza"; 
var name = "sakib"; console.log(name) // sakib // name is just redeclared
```

***吊装进场***

var 和 let/const 组实际上都被提升到各自作用域的顶部，但是它们的声明机制完全不同。我们可以在代码中初始化任何“var”类型变量之前访问它(尽管它返回 undefined)。让我们理解一个关于提升的简单规则，尽管提升需要一个单独的博客来详细讨论。

*   每当代码开始运行时，每个变量声明都会被提升到各自作用域的顶部。在第一行，变量被引入内存。
*   下一个任务是将变量设置为“undefined”作为初始值。位置实际上取决于我们提升的是 var 类型的变量还是 let/const 类型的变量。对于 var 的情况，它设置在第一行之后。对于 let/const，它设置在代码中实际定义变量的那一行。
*   最后一项任务是将实际值赋给变量，如果它是用。对于 var 和 let/const 两种情况，都是在主代码中实际定义和初始化变量的那一行完成的。

让我们用例子来理解这个。

```
console.log(x) // undefined . not error.
 var x=5;
```

2.

```
console.log(x) // throws reference error; 
let x=6;
```

为了便于理解，我们可以假设第一个代码片段在后台被转换成类似这样的内容。

```
1.just tells the memory to assign a block for x. no value would be set. 
2.x = undefined; 
3.console.log(x) // undefined; 
4.x = 5
```

x 被吊到了瞄准镜的顶部。它被引入记忆，但至今没有被赋予任何价值。在下一行中，它被设置为“未定义”。实际值附加在主代码中实际初始化“x”的那一行。

但是在 let 和 const 的情况下，我们可以假设提升和声明是这样工作的。

```
1\. just tells the memory to assign a block for x. 
no value would be set. 2\. console.log(x) // throws reference error. It hasn't been initialized yet. but javascript engine recognizes "x". 3\. x= undefined; 
4\. x= 5;
```

“let”类型变量也被挂起。但是值“undefined”被设置在主代码中实际定义它的行中。因此，如果我们在初始化之前控制台记录它，就没有机会看到“未定义”。实际值将在此之后立即赋值。

*【注意:许多开发人员理解这种提升方式，即使引擎运行 javascript 代码时没有这种机制。要了解提升的实际情况，我们应该了解“执行上下文”。我很快会在后面的博客中谈到这一点】。*

我们已经看到了 let 和 const 如何给我们更多可预测的结果。这就是 Ecmapscript2015 向我们介绍这两款改变游戏规则的玩家的原因。借助 let/const 声明，我们还可以确保代码中任何块和函数的自包含行为。例如:

```
let a = 5; 
{ let a = 10; 
console.log(a) // returns 10 
} 
console.log(a) // returns 5
```

这真的是可以预见的，对吧？我们刚在两个地方取消了 a 的警戒。a=5 在全局范围内。我们还在一个块中声明了“a ”,并期望它与前一个不同。结果还好。

但是如果我们尝试用“var”来模仿类似这样的东西，我们来看看会发生什么！

```
var a= 5 ;
{ 
var a =7;
 console.log(a); // returns 7
 } 
console.log(a) // returns 7
```

如前所述，变量“a”具有函数作用域。尽管我们在两个地方声明了“a ”,并期望它们彼此不同，但它们实际上都属于同一个世界/范围。因此，块中声明的" a "是对" a "的一种重新声明。在这样的用例中，这可能会给我们带来不可预知的结果。

一个好奇的头脑肯定会受到一个关键问题的影响，那就是为什么我还没有区分“const”和“let”。原因是，我期望你知道 let 和 const 的核心区别，那就是 frank。来遮挡更多的光线

我们可以在声明后多次给 let 类型变量赋值。但是我们不能用“const”来做到这一点，因为它的本性是不可改变的。

```
let a = 5;
 a = 7 // this is valid
```

但是，

```
const a =5;
 a =7 // this is not valid. throws syntax error.
```

关于带“object”的 const 类型声明，有一个君子提醒。先看代码。

```
const person =
 { 
name :"reza",
 age : 25 
} 
person.name = "sakib"; // valid; 
person.age =28; // valid;
```

我们实际上可以改变一个常量类型对象的属性。我们不能做的是，在用 const 定义了某个东西之后，我们不能再次重用“person”来指代它。

# 良好做法

*   不要在你的代码中混淆“var”和“let”。坚持一个。
*   尽管 let 和 const 最终会被转换为“var ”(通过 transcompiler ),但在代码中尽可能多地使用 let 和 const 是一个好习惯。在 React 等现代 javascript 库中，开发人员几乎从不在代码中使用“var”。
*   停止创建没有任何关键字的全局变量。如果在 main 函数中声明任何带有 var 的东西，它会自动附加到 window 对象上。如果你希望它是全局的，你也可以在主作用域中声明任何带有“let”和“const”的东西。在这种情况下，它不会附加到窗口对象，但仍然显示相同的全局行为。此外，它以可预测的方式工作。

最后，在理解用 var、let 和 const 声明变量的内部机制时，考虑提升和作用域是很重要的。我将在另一篇博客中更详细地讨论吊装。写这篇博客是为了给你一个 javascript 变量声明的基础，这样你就可以解决任何关于 let、var 和 const 的混淆。现在，用下面的例子挑战自己，不运行代码，尝试猜测输出。如果你在理解它们方面有困难，欢迎在下面评论或者通过[rezaink1996@gmail.com](mailto:rezaink1996@gmail.com)联系我。

*【提示:首先理解变量的范围，然后应用上一节提到的提升规则】*

1.

```
var x = 5; 
if(x===5){ 
let x =7;
 }
 console.log(x) what will be the output ? 
```

2.

```
var x = "game";
 function demo (){ if(x==="game"){ x="cricket" 
} else if (x==="music"){
 x = "rock"
 } else{
 x = "javascript";
 } console.log(x)
 var x ="music"; } demo() // what will be printed ?
```

总结一下，我在上面一节留了一个问题没有回答，并承诺你应该可以自己回答。让我在评论区知道我是否遵守了我的承诺。