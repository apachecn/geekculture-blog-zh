# 什么是复试？

> 原文：<https://medium.com/geekculture/what-the-heck-are-callbacks-75c7d2ea2793?source=collection_archive---------30----------------------->

![](img/beafe845a7467fc2555e1656fa5d7f3e.png)

Photo by [Janez Podnar](https://www.pexels.com/@podnar2018?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/photo-of-brown-house-near-mountain-1424246/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

我相信现在你已经听说过回调函数了。我经常听到的一句话是“把它当作回调来传递”。我总是对这些术语感到困惑，在阅读了这个主题后，它比我想象的要简单得多。回调只是一个作为参数传递给另一个函数的函数。

先说个例子。让我们创建一个函数，然后将它作为参数传递。参见下面的代码:

A callback in Javascript

如您所见，我们设置了一个非常简单的回调。让我们来看看这是怎么回事。

1.  我们设置了一个名为`theText`的函数，它将接受一个参数`text`。我们将`console.log`的`text`是什么。
2.  我们设置了第二个名为`printText`的函数，它接受一个参数`ourCallback`。在这个函数中，我们声明了一个带有字符串值的变量。这个函数的最后一行是调用函数`ourCallback`，我们会传入一个文本的参数。
3.  最后，我们调用`printText`变量，它的参数是我们的第一个函数。这是我们实际的代码回调部分。我们的函数`printText`正在调用函数`theText`。在本例中，它正在执行`This is the text for our callback`中的`console.log`。

很酷的东西。

现在来看看不利的一面。当我们开始嵌套代码时，它会很快变得混乱，正如你在下面看到的

Callback hell is the worst level of hell

既然回调地狱是一个没有开发者想去的地方，我们能做些什么来解决这个问题呢？嗯，我们可以试着让我们的代码非常模块化和更小。我们也可以用新的 JavaScript 编写我们的代码，因为已经有了解决方案。我们现在可以使用 Promises 或 Async Await，因为两者都更简洁且异步。

因为我觉得他们都应该有自己的文章，我会链接到 w3schools 和他们对这个话题的解释，因为他们总是做得很好。

1.  [承诺](https://www.w3schools.com/js/js_promise.asp)
2.  [异步等待](https://www.w3schools.com/js/js_async.asp)

A Promise

Async Await

我发现两者都具有同等的可读性，但是当我亲自编写代码时，我更倾向于 Async Await。我觉得这样看起来更舒服，也不会像一连串的承诺那样乱七八糟。

如果你想看看我的其他帖子，可以在这里找到。我写的都是前端特有的东西，所以我在 [React](https://jgrice01.medium.com/react-basics-lets-create-a-class-based-component-part-1-of-2-7249f7fac75e) ， [TypeScript](https://jgrice01.medium.com/typescript-understanding-the-basics-a2264759cd2d) ，[SASS](/codex/writing-better-sass-with-dynamic-class-generators-e486a0413d0d)&[electronic](https://jgrice01.medium.com/want-to-build-desktop-apps-using-js-say-hello-to-electron-4f862c3b4e38)上有帖子。

我希望这篇文章是令人愉快的。如果你有任何反馈，发表评论，让我知道我可以改进的地方。感谢您的阅读，祝您愉快！