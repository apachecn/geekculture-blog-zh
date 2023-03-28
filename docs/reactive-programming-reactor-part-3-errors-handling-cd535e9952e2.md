# 反应式编程(反应堆)-第 3 部分-错误处理

> 原文：<https://medium.com/geekculture/reactive-programming-reactor-part-3-errors-handling-cd535e9952e2?source=collection_archive---------1----------------------->

当操作数据时，错误是不可避免的，当错误发生时，您必须确保您的代码以某种方式处理它，并优雅地从中恢复。

Reactor 提供了许多操作符来处理异常和错误。

## onErrorResume

当发生任何错误时，如果我们希望订阅后备发布服务器，则使用 onErrorResume。这就像，听着，如果发生任何错误，我希望你忘记上游和发生了什么，继续订阅这个流，我是…