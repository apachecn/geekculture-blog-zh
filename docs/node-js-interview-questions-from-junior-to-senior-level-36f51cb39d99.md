# Node.js 面试问题从初级到高级

> 原文：<https://medium.com/geekculture/node-js-interview-questions-from-junior-to-senior-level-36f51cb39d99?source=collection_archive---------10----------------------->

即使是经验丰富的开发人员也需要在面试前更新知识。当然，每个面试官都有自己的调查问卷，但是有很多共同的问题。在这篇文章中，我准备了一个关于 Node.js 的常见问题列表。让我们开始吧！

![](img/80f84eda38aa1d123388d9672d26c448.png)

# **Node.js**

第一部分是关于 Node.js 本身，而不是 JavaScript。不幸的是，很多人不明白其中的区别。

**node . js 是什么？**

[https://en.wikipedia.org/wiki/Node.js](https://en.wikipedia.org/wiki/Node.js)。Node.js 不是一种编程语言，它是运行 JavaScript 的运行时环境。它使用谷歌的 V8 引擎，谷歌 Chrome 也使用相同的引擎。但是 V8 不是 Node.js 的一部分，它是一个独立的组件。微软试图用他们的 Chakra 引擎分叉 Node.js，但根据 project 的 github，它已经不再维护了。

**Node.js 架构**

[https://codezup . com/node-architecture-V8-javascript-engine-libuv-library/](https://codezup.com/node-architecture-v8-javascript-engine-libuv-library/)V8 将 JavaScript 转换成机器码，libuv 异步执行 IO 操作。

**什么是异步 IO 或者非阻塞 IO？**

[https://en.wikipedia.org/wiki/Asynchronous_I/O](https://en.wikipedia.org/wiki/Asynchronous_I/O)node . js 不会等到 IO 操作完成，除非你使用`fs`中的 sync 方法。异步操作的结果由回调函数处理。

**阻塞和异步 IO 有什么区别？**

许多框架使用阻塞 IO，它们使用线程池来执行并发操作。这种方法的主要缺点是线程开销。每个线程需要≥ 1MB 的内存，这意味着线程的数量受到服务器的限制。如果我们在讨论某个 web 服务器应用程序，那么阻塞 web 服务器对每个请求至少使用一个线程。如果所有线程都忙于等待数据库查询响应或其他事情，新的传入请求必须等待空闲线程。在异步 web 服务器中，当所有需要的回调完成时，传入的请求将收到它们的响应。Nginx 和 Golang 也使用这种方法。

**node . js 有什么用？**

对于具有大量 IO 操作的应用程序，如 API 服务器和实时服务器。对于 CPU 密集型操作，Node.js 不是一个好的选择，因为它们阻塞了 EventLoop。

**什么是 EventLoop？**

[https://nodejs . org/en/docs/guides/event-loop-timers-and-next tick/](https://nodejs.org/en/docs/guides/event-loop-timers-and-nexttick/)你需要记住 EventLoop 的 6 个阶段。面试官也可能会问`setTimeout`、`setImmediate`和`process.nextTick`的情况。写一个简单的脚本并检查它们的执行顺序，它会帮助你记住这个问题。

**node . js 是单线程的吗？**

是也不是，JavaScript 操作在一个线程中执行，开发人员不用担心并发安全，`libuv`与多个线程一起工作[https://medium . com/@ Flos loot/node-js-is-not-single-threaded-88928 ada 5838](/@FloSloot/node-js-is-not-single-threaded-88928ada5838)。

Node.js 还引入了 worker threads[https://nodejs . org/docs/latest-v 14 . x/API/worker _ threads . html](https://nodejs.org/docs/latest-v14.x/api/worker_threads.html)

**如何在不阻塞 EventLoop 的情况下执行 CPU 密集型操作？**

有几个选项可以做到这一点:

1.  编写一个单独的服务来执行此操作。此外，单独的服务可以用更适合这个任务的其他语言编写。
2.  如果可能的话，将操作分成多个小任务，然后用`setImmediate`运行下一个任务。
3.  使用工作线程。

**计时器精确吗？**

没有。由于 EventLoop 机制，`setTimeout`和`setInterval`中的回调总是在定义的时间后被调用，有一些延迟。

**垃圾收集**

[https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Memory _ Management](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Memory_Management)和[https://blog . rising stack . com/node-js-at-scale-node-js-garbage-collection/](https://blog.risingstack.com/node-js-at-scale-node-js-garbage-collection/)。如果你想深入了解这个话题，你可以阅读本页[https://github.com/thlorenz/v8-perf/blob/master/gc.md](https://github.com/thlorenz/v8-perf/blob/master/gc.md)

# Java Script 语言

JavaScript 在过去的六年里有了显著的改进。很多新开发人员没有听说过原型，他们使用类，他们很高兴。我完全理解他们。但是有些面试官是老派的人，他们会问老派的问题。

**JavaScript 数据类型**

[https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Data _ structures](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures)我不知道该补充什么，这个文档很好地描述了这个话题。

**数字**

[https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Global _ Objects/Number](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Number)请记住，JavaScript 无法执行精确计算。为此最好使用一些外部库。

**var，const，let 有什么区别？**

[https://www . freecodecamp . org/news/var-let-and-const-whats-the-difference/](https://www.freecodecamp.org/news/var-let-and-const-whats-the-difference/)只要记住什么是吊装以及为什么不应该使用`var`

**箭头功能**

[https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow _ Functions](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

**调用、应用、绑定**

`call`—[https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Global _ Objects/Function/call](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/call)

`apply`—[https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Global _ Objects/Function/apply](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/apply)

`bind`—[https://developer . Mozilla . org/ru/docs/Web/JavaScript/Reference/Global _ Objects/Function/bind](https://developer.mozilla.org/ru/docs/Web/JavaScript/Reference/Global_Objects/Function/bind)

在大多数情况下，这些方法可以用箭头函数代替。

**类**

https://www.w3schools.com/js/js_classes.asp:类只是函数原型的一种甜言蜜语。

**继承**

很多考生回答继承是由`extends`执行的。这是真的，但这只是一个甜言蜜语的语法。本文档将帮助您了解它是如何工作的[https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Inheritance _ and _ the _ prototype _ chain](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Inheritance_and_the_prototype_chain)

**回调地狱**

[http://callbackhell.com/](http://callbackhell.com/)过去，许多开发人员因为回调地狱而讨厌 JavaScript，但是现在 JavaScript 提供了很好的处理方式，比如`Promise`和`async/await`。

**承诺**

[https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Global _ Objects/Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)。Promise 是回调函数及其状态的包装器。它还包含回调函数执行的结果。

**异步/等待**

[https://developer . Mozilla . org/en-US/docs/Web/JavaScript/Reference/Statements/async _ function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/async_function)。实际上，这是另一种甜言蜜语，因为承诺把我们从回调地狱中拯救出来，但却带来了承诺地狱。提示注意:`async/await`不仅可以处理内置承诺，还可以处理任何提供承诺接口的对象。例如 https://www.npmjs.com/package/bluebird 的`bluebird`作品兼容`async/await`的

什么是微任务？

承诺还带来了一个新概念“微任务”[https://JavaScript . info/event-loop # macro tasks-and-micro tasks](https://javascript.info/event-loop#macrotasks-and-microtasks)

今天就到这里吧！当然，面试官可能会问你更多的问题，不仅仅是关于 Node.js 和 JavaScript 的问题，可能是关于 SOLID、设计模式、架构、数据库和许多其他主题的问题。但是如果你知道这篇文章中所有问题的答案，你一定会给你的面试官留下一个好印象。

请随意提问，或者我可能错过了一些话题。只需在下方留下评论。

下次见！Servus！