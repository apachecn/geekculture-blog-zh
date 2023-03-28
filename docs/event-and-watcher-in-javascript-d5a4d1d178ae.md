# JavaScript 中的事件和观察器

> 原文：<https://medium.com/geekculture/event-and-watcher-in-javascript-d5a4d1d178ae?source=collection_archive---------20----------------------->

![](img/8a1b5083b63ac7bec0abc3d204965de3.png)

在当今的编程中，我们总是需要一些代码段，这些代码段能够自动发出并执行任务，没有任何中断，并且能够处理所有已定义的场景。

每个人都听说过事件和观察者，但是你们中有多少人知道它们之间的区别以及它们可以用在哪里？

简单来说，我们可以调用[事件](https://www.npmjs.com/package/events)就像线程中的函数一样，就像我们调用它们时它们被执行的函数一样。事件在被触发时被执行，然后在一个线程中被执行。

## 然而什么是[观察者](https://www.npmjs.com/package/watcher)？

![](img/afedc68a27acb3771a1e26a36414867d.png)

观察者是一种特殊的动物。js 特性，允许您监视组件状态的一个属性，并在属性值改变时运行一个函数。

或者简单地说，观察器用来监视任何特定的预先记录的数据，如数据库。一个观察者就像一个看守人，他密切关注任何更新或变化，无论何时发生，他都会调用一个处理程序来处理这些变化。

在调用定义之后，让我们在通过 npm 安装它之前看看它的外观和工作方式，同样遵循上面的文档！

这是事件定义的样子…

```
@OnEvent(NAME_OF_EVENT, { async: true }) async handle(PARAMETER: DATA_TYPE) { try { // something.... which does it work } catch (e) { // catch the error and log }}
```

现在让我们看看如何触发一个事件……
1。首先，我们需要导入事件发射器。
2。使用事件发射器—发射我们想要触发的事件，并传递参数(如果有)。

```
this.EventEmitter.emit(NAME_OF_EVENT, PARAMETER_TO_PASSED);
```

现在让我们把注意力放在观察器上，它通常按照定义监视数据库中的更改/更新。

```
private updateTestListener() { let filter = { $match: { $and: [{<QUERY>}, { $or: [{ operationType: "update" }] } ] } };this.<Schema_Model>.watch([filter], { fullDocument: "updateLookup" }).on("change", async doc => this.handleUpdates(doc));}async handleUpdates(doc) { const { operationType, fullDocument: {<PARTICULAR_FIELD>} } = doc; // this function does what it suppose to do on fields which is      got updated... // we can even collect the only updated item on which we need to perform something.}
```

两者都是可注入的功能，在 Provider 中提到它们之后，可以在任何模块和类中使用(更多信息请查看 [Nest Js](https://docs.nestjs.com/) doc)。

# 结论

我们研究了 javascript 事件和观察器，发现了许多有助于我们弄清楚发生了什么的信息。这篇文章仅仅是一个想法的总结，你可以在这个永无止境的世界中探索更多。