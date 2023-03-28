# 发明游戏。JavaScript 中的游戏循环。

> 原文：<https://medium.com/geekculture/inventing-games-game-loop-in-javascript-1cb0a5fcc81b?source=collection_archive---------16----------------------->

在[之前的帖子](/geekculture/inventing-games-game-loop-a53a66cf874b)中，我以一个简单的控制台应用程序为例解释了什么是游戏循环，现在我将向你展示如何在没有任何第三方解决方案的情况下借助 JavaScript 构建游戏循环。

![](img/a24c033d9bee1725726a482d16532083.png)

Photo by [Pixabay](https://www.pexels.com/@pixabay?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/turned-on-red-and-green-nintendo-switch-371924/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

# JS 中的循环。

在 JavasCript 中有几种组织循环的可能性，最常见的有:

*   for 语句
*   do … while 语句
*   while 语句

他们都很有名，因此，我想，我不会在这里描述他们。我真正想指出的是，这些陈述并不完全符合游戏开发的需求。有很多东西应该围绕这些语句手动构建；其中最主要的是帧同步(关于同步的更多细节请参见计算机图形学文章)。

[](https://antarh.medium.com/computer-graphics-frame-rate-48967ec615d) [## 计算机图形学。帧速率。

### 当你通过代码创建一个动画时，你可能会遇到跳帧或者画面闪烁的情况。这个故事旨在…

antarh.medium.com](https://antarh.medium.com/computer-graphics-frame-rate-48967ec615d) 

如果默认语言循环帮不了我们，我们该怎么办？答案是:使用 setInterval 和 requestAnimationFrame 函数。

# setInterval 函数。

关于这个函数我们应该了解什么？

> 窗口和工作接口上提供的 setInterval()方法重复调用一个函数或执行一段代码，每次调用之间有固定的时间延迟
> 
> **— MDN 网络文档。**

`Fixed time delay between each call`听起来很神奇！这意味着我们可以设置帧速率，并确保每个调用将以相同的时间间隔重新绘制游戏状态。让我们看看如何在它的帮助下创建一个游戏循环:

Set Interval Example.

酷，我们完成了！但是等等……让我想想，`1000 / 60 = 1.6666666666666666... ms`那不等于 1/60 秒。它将被舍入到某个最接近的值，但不相同。此外，如果您的代码需要完成的不止是`1.6666666666666666... ms`,那么前一个循环一完成，下一个循环就会运行。这样的话，帧率会受到影响。为了检查这种行为，我建议创建一个简单的 html 页面并添加以下内容。

FPS Counter.

如果一切正常，您应该会看到显示帧速率的随机数。如果你仔细观察，你会发现 60 fps 是罕见的情况，这意味着这种方法可能会导致 flikers 和帧跳过，正如上面提到的计算机图形文章中所讨论的那样。此外，我们需要考虑如何同步同一页面上的几个动画，每个动画都在自己的 setInterval 函数中运行。

怎么处理？不行。因为不需要，JavaScript 提供了另一个神奇的函数，叫做 requestAnimationFrame。

# requestAnimationFrame 函数。

> window.requestAnimationFrame()方法告诉浏览器您希望执行动画，并请求浏览器在下一次重画之前调用指定的函数来更新动画。
> 
> **— MDN 网络文档。**

我反复阅读了这个定义几次，每次它的意思都消失了。为了更好地理解它，让我们在这里深入探讨一下。

在浏览器中打开的每个页面都有一个与浏览上下文相关联的文档。每个文档都有一个动画帧回调列表和单个动画帧回调标识符。最初，列表是空的，标识符等于零。当我们在代码中定义 requestAnimationFrame 函数时，我们传递一个回调作为函数的参数。该回调被推送到动画帧回调列表。在定义中，这个过程被称为`tells the browser that you wish to perform an animation`。

同时，浏览器定期安排一个**单个**任务，对浏览器环境中的所有动画进行采样。该任务循环遍历每个页面，并检查其可见性属性。如果一个页面是可见的，这些任务会查看动画帧回调列表，并并排调用它们。如果有许多回调或 CPU 负载过重，浏览器可以选择这样的帧速率，以便所有的动画可以尽可能平稳地运行。任务完成后，动画帧回调标识符加 1。在定义中，这个过程被称为`the browser call a specified function to update an animation before the next repaint`。

你看到该功能提供的所有好处了吗？

*   我们不需要担心帧同步
*   所有动画将同时运行
*   如果我们的页面现在不是活动的，动画将不会运行

让我们看看它是如何工作的。为此，我们需要删除 setInterval 函数，并重新编写我们的 draw 方法。

Draw Function with `requestAnimationFrame Function`

理想情况下，该示例在正常状态下应该显示 60 FPS。但是，根据您的浏览器和 CPU 负载，结果可能会有所不同。

# 结论。

因此，正如我们所见，在 JavaScript 中组织游戏循环的最佳方式是利用 requestAnimationFrame，如下面的代码片段所示:

Game Loop

很简单，你不觉得吗？

感谢您的阅读。我希望你喜欢这个故事。你真诚的，亚历克斯。