# JS 编程中的匿名函数

> 原文：<https://medium.com/geekculture/anonymous-functions-in-programming-765ee724c714?source=collection_archive---------29----------------------->

他们比看起来更有吸引力。

![](img/aca1a4663477ec15dd6cca95b25ea7e9.png)

Photo by [Tarik Haiga](https://unsplash.com/@tar1k?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/anonymous?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

多年来，JavaScript 一直在快速更新。它的生态系统正在快速扩张。所以，最好掌握它们的知识，更新。我个人认为匿名函数非常有趣，在很多情况下都很有用。嗯，JS 库中非常推荐它们，它们在使代码更加紧凑、简洁和可读方面起着关键作用。

因此，让我们深入研究一下，快速浏览一下它们的样子，以及它们如何在各种场景中使用。*这篇文章是为那些不熟悉匿名函数的人准备的*。

现在，从哪里开始？
我们有一种语言，实际上统治着网络及其辅助技术。当谈到 Web 开发时，JavaScript 证明了自己是不可战胜和不可避免的，尽管有像 Django 和 Flask 这样的库使用 Python 来完成这项工作。尽管如此，JavaScript 才是最终真正呈现在网络上的。这只是开发社区喜欢的其他编码方式，也是基于他们以前的经验和对特定编程或脚本语言的兴趣。所以，我们开始了，JavaScript 和它的匿名函数的概念！

***基本上，匿名函数就是一个没有名字的函数，就像一个没有名字或者 id 的匿名用户，特别要被跟踪。它就在那里，就是这样。它有助于完成工作，并不真正关心在序列的后面出现自己。***

**基本语法:** function() {
//函数体。
}

因此，这种类型的函数没有名称，如上例所示。但是，如果没有引用(名称)，如何在代码中使用它呢？嗯，这就是 JS 的其他特性发挥作用的地方。我们可以根据需要在多个实例中使用它们，以保持代码的整洁。

[***life(立即调用函数表达式)***](https://thenikhilmannem.medium.com/immediately-invoked-function-expression-iife-in-javascript-9c3d73f33811)所以，当我想到在哪里使用匿名函数时，首先想到的就是[life](https://thenikhilmannem.medium.com/immediately-invoked-function-expression-iife-in-javascript-9c3d73f33811)，一个 JS 习语。如果您是 JS 代码的新手，这可能会让人觉得奇怪，但是，在您不想重用代码片段(可能是一些需要运行的逻辑，但是在流程的后面不需要)的情况下，这非常有用。

**life 语法:**
(function() {
//函数体。
})()；

因此，正如你所看到的，语法只是在函数周围和旁边添加了一对括号，以一种有效的方式。就是这样。它的意思是，你将一个匿名函数封装在括号内，然后像调用任何其他函数一样调用它，即在封装的匿名函数旁边使用括号。

这一概念适用于这种情况，只要您希望某些代码在它所在的位置执行，而不管它周围的其他代码的功能或流程。就我个人而言，我强烈建议记下它，这样当你在特定情况下使用它时，你会发现它是多么有用。

***使用匿名函数作为一个函数的值***

因此，我们可以使用匿名函数作为赋给变量的值:

let some function = function()= > {
//函数体。
}；

上面的例子演示了匿名函数被赋给一个变量。在 JavaScript 中，函数可以通过多种方式创建。毕竟，它基本上是为函数式编程范式设计的语言。
一般来说，函数是直接在 JS 代码中定义的，在执行周围的块或脚本时声明。但是，这种通过将函数赋给变量来定义函数的方法，在到达被赋值的那一行之前，不会声明一个函数。
这意味着分配给变量的函数在声明之前不可用，并且直到加载和/或执行脚本或其周围的代码块之后才可用。

***在中途使用匿名函数***

我们也可以在函数中使用匿名函数， [arrow](https://codeburst.io/arrow-functions-in-javascript-49079715e993) 或其他。在你需要一段逻辑发生的函数中使用它，而不用担心以后会发生，这可能是一件好事。你可能一辈子都要用到它。

**例如:**

let someFunction = () => {
//函数体。
//内部函数
(() = > {
//内部函数体。
})()；
}

这些是在 JS 中使用匿名函数的一些想法。如果我遇到更多的用法和/或实现，我会添加更多。

谢谢你一直忍耐到文章结束。我希望这能在任何可能的方面有所帮助。

长码和繁荣🖖，你们所有人！

问候，
[Nikhil Mannem](http://thenikhilmannem.com) ！