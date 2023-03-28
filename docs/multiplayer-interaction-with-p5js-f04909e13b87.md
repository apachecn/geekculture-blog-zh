# 与 p5js 的多人互动

> 原文：<https://medium.com/geekculture/multiplayer-interaction-with-p5js-f04909e13b87?source=collection_archive---------14----------------------->

## 在这篇文章中，我将描述使用 [p5js](https://p5js.org/) 、 [socket.io](https://socket.io/) 和 [express](https://expressjs.com/) 创建多人互动网站/游戏的基本设置。

![](img/0df9b8bd5c7eb6feb4b8052056df9172.png)

Screenshot of the video on my interactive website, reacting to the positions of participants interacting on my website.

这篇文章的目标是建立一个基本的系统，从这个系统你可以扩展创建你自己的多人互动/游戏。我们将讨论如何在客户端和服务器上使用 socket.io 来不断更新 p5js 中每个客户端的位置，这些位置可以代表一个玩游戏的玩家或与网站交互的人。

# 计算机网络服务器

本教程假设您对 node.js 和 npm 有一些基本的了解。如果没有，尝试按照本教程先安装并获得 npm 的基本知识。

我们只需要服务器上的两个依赖项:`npm install express socket.io`

这里发生了很多事情，所以让我们把它分解一下。

服务器代码的第一部分是导入 express、http 和 socket.io。

Express 用于创建主服务器，用户可以在那里访问一个 url，它将返回一些内容。在这种情况下，我将它设置为当有人转到我网站的根目录时返回“index.html”。

```
app.get(“/”, (req, res) => { res.render(‘index.html’); });
```

socket.io 是一个库，提供从每个客户端到服务器的实时更新。仅仅使用 express 服务器是无法做到这一点的，因为标准请求的处理时间太长。相反，socket.io 主要在幕后使用 W [ebSockets](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets_API) ，这使得客户端、服务器和其他客户端之间的实时通信成为可能。

为了存储每个访问我网站的客户的位置，我只使用了一个普通的 javascript 对象。`const positions = {}`每当新客户端通过访问我的网站进行连接时，就会触发“连接”事件。它们将被赋予一个唯一的 id，作为该对象中的一个键。例如，如果 1 个客户端连接到位置对象，则该对象将如下所示:

```
{ unique_id1: {x: 0.5, y: 0.5} }
```

当第二个客户端连接时…

```
{ 
  unique_id1: {x: 0.5, y: 0.5}
  unique_id2: {x: 0.5, y: 0.5}
}
```

例如，如果第一个人离开网站，该 id 将被删除，对象将如下所示:

```
{ unique_id2: {x: 0.5, y: 0.5} }
```

从客户端，人们可以用消息向服务器发送数据。在这种情况下，我们可以从客户端接收“updatePosition ”,这将更新该客户端的位置。例如，如果客户端 2 使用数据:`{ x: 0.2, y: 0.6 }`调用“updatePosition”，它会将 positions 对象更新为:

```
{ unique_id2: {x: 0.2, y: 0.6 }
```

最后，我们将每个客户的位置发送给所有相关的人。我们可以用`io.emit('positions', positions);`行来实现。它向每个客户端发送名为“positions”的消息以及 positions 对象。我们将它打包到一个`setInterval`中，这样位置每秒发送 30 次。

现在我们有了基本的服务器代码，我们可以开始查看客户端，以及如何设置网站以连接到服务器，并发送和接收与它们在我们将使用 p5js 创建的画布上的位置相关的消息。我们将从用 p5js 设置画布和定位开始，然后添加 socket.io 用法。

# 客户端(p5js)

首先，我设置了一个导入代码的基本 html 页面，这样我们就可以在 javascript 文件中使用 **socket.io** 和 **p5js** 。它还设置了一些基本的样式，所以我们的`canvas`元素将总是相同的长宽比，并充满屏幕。

在`<div id='sketch-container'>`里面 p5js 会放画布。我们将在结束标签`body`之前的索引文件的最后部分包含的`script.js`中设置这一切。

现在，我们可以在画布中设置 p5js，首先是代码，我们将在下面查看它:

这段代码建立了一个非常基本的 p5 画布，它在画布的左上角画了一个圆。当客户端在画布上单击时，圆圈的位置会变为用户单击的位置。

注意，我们在[实例模式](https://p5js.org/examples/instance-mode-instantiation.html)中使用 p5js。这就是为什么我首先设置了一个将 p5js 作为参数的`function sketch`。虽然 p5js 将此描述为使用 p5 的一种更高级的方式，但我发现这更容易，因为您可以很容易地看到哪些函数来自 p5，因为所有的函数现在都绑定到 sketch 函数中给出的参数，所以我们调用`p.setup`和`p.draw`等..该函数立即在第`new p5(sketch, sketchContainer);`行被调用。

## 从服务器获取位置

和以前一样，让我们先看看代码，下面我们将讨论一些感兴趣的要点。

在我们最后的`script.js`文件中，我们现在用行`const socket = io();`连接到服务器，我们初始化它只使用 websockets 作为连接，就像在服务器上一样。这使得使用旧浏览器的人无法使用我们的应用程序，但它确保了客户端和服务器之间的所有通信都是快速的，并且使用更少的带宽。

在我们的`sketch`函数中，我们现在有了一个`let positions = {}`对象，而不仅仅是一个位置。在这里，我们将存储连接到我们网站的每个客户的位置。我们通过用`socket.on("positions", callback);`函数设置回调来获取位置。在回调中，我们接收数据，并简单地将 positions 对象设置为来自服务器的数据。请记住，这个“位置”消息每秒发送 30 次，因此我们的数据将不断更新。

在我们的 draw 函数中，我们现在也要做一些改变。我们希望显示所有的圆圈，而不是一个，所以我们用一个`for...in`循环遍历 positions 对象中的每个关键点。然后，对于每个位置，我们绘制我们的圆，注意我们现在必须在 draw 函数中将位置乘以宽度和高度，因为每个位置只是 0 和 1 之间的一个数字。

*这是必要的，因为我们无法在服务器上存储实际位置。例如，当一个客户在一个画布只有 500 像素宽的小屏幕上时，如果他们的 x 位置是中间位置(250 像素)并存储在服务器上，它将只出现在观看宽度为 1000 像素的网站的人的四分之一处。*

最后，我们必须调整我们的`cnv.mousePressed`功能。我们想通过用`updatePosition`发送新的位置到服务器来更新我们的位置。然后，服务器会通过我们之前已经设置好的`“positions”`消息监听器将更新后的位置发送给我们和其他人。(连续发送位置的那个)。

出于上述同样的原因，我们必须存储相对于服务器的位置值，而不是绝对像素值，所以我们用`p.mouseX`除以`p.width`，对于高度也是如此。

就是这样！现在你有了一个实验多人互动或游戏的基本设置。从这里开始，你可以根据自己的喜好扩展代码，例如，你可以开始考虑给代表“我”的圆赋予不同的颜色或大小，这可以通过从服务器中查找套接字 id 来完成，可以使用`socket.io.engine.id;`来获得，然后将它与在`positions`对象中使用的所有 id 进行比较。

# 完整项目

Multiplayer positioning with socket.io and p5js

感谢您的阅读！欢迎提出问题或建议！如果你感兴趣，这里有一个我用这个基本设置制作的艺术项目的视频和一个实时视频，它也对每个人的位置做出反应。