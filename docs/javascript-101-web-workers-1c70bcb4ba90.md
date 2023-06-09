# Javascript 101:网络工作者

> 原文：<https://medium.com/geekculture/javascript-101-web-workers-1c70bcb4ba90?source=collection_archive---------12----------------------->

![](img/b0e1b276710df6ff42ef59e9b205a855.png)

Javascript 是单线程语言，这意味着它一次只能处理一个任务。为了处理多任务，Javascript 在任务之间切换，就像单核机器通过一个称为并发性的概念来处理一样——虚拟并行性。

这种模式虽然对于非阻塞任务来说是安全的，但对于大量资源需求的程序(例如客户端上的音频压缩和声音转换)来说却是一场噩梦，这些程序大多数时候需要同时运行(**并行**)，而不仅仅是与主应用程序有重叠的时间段(**并发**)。这就是网络工作者的用武之地。

# 什么是网络工作者？

> Web Worker 是浏览器(也称为主机环境)的一个特性，它提供了 Javascript 引擎的多个实例，允许我们在不同的线程中运行不同的程序。

![](img/432955bbfd9df7dd51fc952663236f9c.png)

web worker 可以如上所示进行初始化，浏览器将启动一个单独的线程，让 worker 文件作为一个独立的程序运行。这种类型的并行性被称为“任务并行性”，因为它强调的是将程序的各个部分分开来并行运行。

> 注意——JavaScript 仍然不支持多线程执行。但是通过浏览器，网络工作者让我们有能力。

# **工人环境**

Worker 无权访问页面的 DOM，但拥有 navigator、location、JSON 和 applicationCache 变量的副本，并且可以执行网络操作(Ajax、Websockets)和设置计时器操作。

![](img/68db3a8efad9c37d0256b96387ce1565.png)

**w1** Worker 对象是一个事件监听器和触发器，它允许您订阅由 Worker 发送的事件以及向 Worker 发送事件，如上所示；

![](img/43f19f088579b4f40afb7577764a6b47.png)

同时，在上面的 worker 文件中，我们监听消息事件，并从事件参数的 data 属性访问传输的有效负载。我们看到的所有片段的组合将创建一个专用的 worker 文件，该文件将在 5000 毫秒后输出*“完成的严肃计算”*，此时主应用程序可以执行其他活动，而不会阻塞其单线程。

这个 web workers 的例子虽然不够实用，但却描绘了其他较重的进程如何最好地在浏览器上处理的图像；处理密集的数学计算、分类大型数据集、数据操作(压缩、音频分析、图像像素操作等)。)

# 共享工人

随着浏览器选项卡间共享资源需求的增长，让一个工作人员专门负责主应用程序变得有些笨拙。为同一应用程序的每个浏览器实例重新创建它们的想法似乎是多余的。这就是共享工作者的用武之地。

> 共享工作器是一种可以从多个浏览上下文访问的工作器:窗口、iframes 或其他工作器

![](img/340a44f0ac1e10d5e07cf23b7c63c770.png)

因为共享工作器可以连接到或来自站点上的多个程序实例或页面，所以工作器需要一种方法来知道消息来自哪个程序。这个唯一的标识被称为“端口”——想想网络套接字端口。所以调用程序必须使用 Worker 的端口对象进行通信。

![](img/b5534a70601a301da6a6a90e9507c2fa.png)

在共享工作者内部，必须处理一个额外的事件:“连接”。此事件为该特定连接提供端口对象。除了这些细微差别之外，共享和专用工人实际上有相同的实现。

# 结论

Web workers 是一项令人兴奋的技术，如果使用得当，它可以在浏览器中提供 javascript 的多线程感觉。它在网页之外的灵活性为可以作为服务执行的特性(推送通知、后台同步等)打开了大门，并且大多数时候不需要用户交互。

下一站— [事件](https://caleb-codes.medium.com/javascript-101-events-9c90fb7e2dce)

# 参考

*   [你不知道的 JS: Async &性能](https://www.amazon.ca/You-Dont-Know-JS-Performance/dp/1491904224)