# 如何在各种场景中处理 javascript 承诺

> 原文：<https://medium.com/geekculture/how-to-handle-javascript-promise-in-various-scenario-3e47959fe80a?source=collection_archive---------44----------------------->

在 [**虚拟赛伯龙私人有限公司**](http://cybertrons.in/) **做项目的时候。**，我在处理 javascript 场景时遇到了困难，它要求代码以同步方式执行，并为此使用承诺。

因为没有合适的解决方法，所以我决定自己做。我将逐一讨论这些场景。

让我们从场景 1 开始，在这个场景中，我们必须不止一次地调用一个函数(不返回任何东西)，以同步的方式一个接一个地调用。

Code snippet with problem for scenario 1

**预期输出:** 102 与 308 之和:
410
-1 与 38 之和:
37

**实际输出:** 102 和 308 之和:
-1 和 38 之和:
410
37

实际输出与预期不同，因为代码以异步方式执行。上面的代码可以用 Promise 解决。下面是处理它的正确代码。

Code Snippet with solution for scenario 1

让我们转到第二个场景，在这个场景中，我们必须不止一次地调用一个函数(返回一些东西)，以同步的方式一个接一个地调用。

Code Snippet with problem to be resolved

**预期输出:**
102 和 308 之和:410
1 和 38 之和:37

**实际输出:**
102 和 308 之和:未定义
-1 和 38 之和:未定义

同样，实际输出与预期不同，因为代码以异步方式执行。上面的代码可以用 Promise 解决。下面是处理它的正确代码。

Code Snippet with solution for scenario 2

虽然在上面讨论的两个场景中，我们已经实现了我们想要的工作，但是随着代码复杂性的增加，代码的可读性降低，代码可能变得难以理解。所以为了让代码易于理解，我们可以使用 async-await。

在使用 async-await 解决上述场景之前，理解 async-await 实际上是如何工作的很重要。

**下面是理解异步等待工作的例子。**

Code snippet with misconception

**预期输出:**
调用 someFunction
在 someFunction 完全执行后，使用参数:10
调用了 someFunction

**实际输出:**

正确的做法如下:

Code Snippet with correct way to use async-await

**使用异步等待的场景 1 的解决方案代码:**

**使用异步等待的场景 2 的解决方案代码:**

**注意**:在最后一个代码片段中，调用 resolve 时作为参数传递的内容被分配给 sum1(或下一个调用中的 sum2)。

关于在 async-await 中处理拒绝的更多场景和解决方案将在文章[‘如何在 async-await 中处理 javascript promise 拒绝’](https://yashgovindani.medium.com/how-to-handle-javascript-promise-rejection-in-case-of-async-await-5c9902b50970)中讨论。
谢谢你。