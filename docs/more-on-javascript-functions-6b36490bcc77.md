# 关于 JavaScript 函数的更多信息

> 原文：<https://medium.com/geekculture/more-on-javascript-functions-6b36490bcc77?source=collection_archive---------31----------------------->

![](img/e574bd8c3cdfef7db1379eca58f3f25c.png)

Photo by [Eric Prouzet](https://unsplash.com/@eprouzet?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**难度**:初学者

在上一篇文章中，我谈到了函数的概念以及如何创建它们。我们定义了它的基本结构和用途。我相信函数是 JavaScript 中最重要的概念之一。在函数的基础上，让我们来看看 JavaScript 中最基本的概念之一:回调或回调函数。

# 什么是回拨？

我必须承认，我在这个概念上挣扎了很长时间，没有看到我付诸实践的东西和我所学到的东西之间的联系。我一直认为“回调”这个名字是误导性的，给你的印象是这是一个神奇的概念；毕竟，谁在给谁回电话呢？这不是魔法，这只是函数的自然组成部分。

我教的是全栈 JavaScript bootcamp，我用 JavaScript 教的第一件事就是函数。我们讨论函数是如何成为一等公民的，以及它们可以像变量一样被传递、被赋值给变量(不仅仅是*结果*，还有实际未被调用的函数】、*以及稍后被调用/调用*。学生们点头表示理解，因为这并不是一个很难理解的概念。然而，当我们总是试图通过使用回调来将这个概念付诸实践时，就好像我在说外语一样。为什么？

> 把回拨功能想象成一个可以在人与人之间传递的电视遥控器。这些人中的任何一个都可以按下(调用)按钮来改变频道。回调只是你传递的一个函数定义。我说，请注意，因为理解这个概念对于理解从普通 JS 中的事件处理程序到 React 中的提升状态都是至关重要的。

使用上面的远程控制类比…假设你的表弟 Bob 正坐在沙发上(他是事件处理者)。鲍勃生活中的唯一目的就是倾听电视室里的人提出的调高音量、换频道等要求。鲍勃是一个很好的听众，但是如果鲍勃没有遥控器，他什么也做不了。所以你，作为一个伟大的主持人，给鲍勃遥控器(功能)。你把它传给 Bob，还没有按下任何按钮(未激活)。当你把未调用的函数交给 Bob 时，根据定义，它就变成了一个回调函数。Bob 现在*有能力*通过使用遥控器调用/调用功能(按下按钮)来响应房间中的人的请求。也许“回调”应该被重新命名为“作为变量传递的未调用的函数定义，以便以后调用”。嗯，也许不是。

如你所见，我非常明确，非常明显，没有任何假设。我个人希望我过去读过的更多文章都是这样的。这会让生活变得更容易学习。即使在现场课堂上，教师也会对基线知识做出某些假设，如果学生不提问/澄清，这一时刻就失去了。我会尽量不在这里这样做。

**简单回调示例:**

```
function bark() {
    return 'woof';
}*function* doDogStuff(*event*, *response*) { if (event === 'intruder'){ console.log(response()); //output 'woof' to console }else { console.log('doing nothing'); // output 'doing nothing' }}doDogStuff('intruder', bark);
doDogStuff('homeowner', bark);
```

在上面这个简单的例子中，bark 是一个返回“woof”的函数，doDogStuff 是一个接受事件参数(在我们的例子中是一个简单的字符串)和响应参数(在我们的例子中，它需要一个表示函数的变量)的函数。

当调用 doDogStuff 时，事件字符串(“入侵者”，或“房主”)和变量“bark”作为参数传入。变量“bark”代表一个未调用的函数。

*这是关键点:bark 是一个没有按键的电视遥控器，它正被传给鲍勃(doDogStuff)，他负责在时机合适的时候按下它。*

我们不知道 bark 函数何时被调用，因为我们不控制它(Bob 控制它)。只有当事件是“入侵者”时，才会调用“吠叫”。还要注意，在 doDogStuff 函数中，“response”是现在保存 bark 函数的变量。所以为了调用 doDogStuff 内部的 bark 函数，您调用 response()。是的，您可以直接调用 bark()，但这不是很好的函数式编程实践。

**通用事件监听器和回调响应:**

让我们尝试一个更常见的例子，一个随处可见的例子。DOM 事件侦听器模式是解释回调时使用的经典示例。如果没有一个更简单的例子，这在一开始对我来说是没有意义的，因此有了电视遥控器的类比和上面这个简单的例子。为了进行设置，我们需要创建一个简单的 html 按钮和一个在 html:

index.html

```
<html> <body> <button id="myButton"></button> </body>
  <script src="./index.js"></script></html>
```

索引. js

```
let myButton = document.getElementById('myButton')myButton.addEventListener('click', response);*function* response() { console.log('I was clicked')}
```

*   addEventListener 函数类似于上面的 doDogStuff 函数
*   “反应”类似于我们上面的吠声函数
*   “response”作为变量传递给 addEventListener
*   当“点击”发生时，我们相信响应回调函数被调用…就像我们的简单例子一样
*   不，您实际上看不到调用，但它确实发生了(它是 DOM 方法的一部分)。我们通过 console.log('我被点击')被执行这一事实看到了结果

这就是它…回调函数最冗长和谦逊的形式。这里的关键是，你定义一个函数并给它一个名字。然后，您可以将该名称(变量)传递到您喜欢的任何地方。然后……某个其他实体，无论是另一个函数还是代码，都可以在适当的时候调用它。

我希望这有助于你理解什么是回调函数。如果你有任何意见，请给我留言。现在…回到编码上来！

上一篇:[什么是 JavaScript 函数？](https://calvincheng919.medium.com/what-are-javascript-functions-bbb9f0157de1)