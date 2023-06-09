# 发明游戏。游戏循环。

> 原文：<https://medium.com/geekculture/inventing-games-game-loop-a53a66cf874b?source=collection_archive---------39----------------------->

# 什么是计算机程序？

你可以说:“好奇怪的问题啊？大家都知道答案！”我同意这种说法，但是你真的知道计算机程序本质上是什么吗？在从事了十年的软件开发和数千行代码之后，我正在重新思考这个问题。

> 计算机程序是由计算机执行时执行特定任务的指令集合。
> 
> **-维基百科**

为了理解这个定义，我建议你回到过去，看看被用作“大铁机器”源代码的 Punhed 卡。穿孔卡片是一种纸板卡片，其孔按列排列，孔的数量因卡片类型而异。最初，卡片上的每个洞可以包含一个单独的是/否问题的答案，但通过允许指示文本的数字和字符的列进一步组织。例如，列中的一个孔可以表示 1，两个孔可以表示 2，依此类推。我不认为我们需要深入细节，只要想象一张卡片代表机器必须做的指令，例如，将一个数字加到另一个数字上。以这种方式，一组卡片被依次装入“大铁机器”中，代表一个计算机程序。这里的要点是程序在加载第一张卡时开始，在处理完最后一张卡时结束。

![](img/fe5a3bda95971f0f80de488379cc8a6b.png)

Photo by [ArnoldReinhold](https://commons.wikimedia.org/wiki/User:ArnoldReinhold). Licence [CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0). No modificatinos.

你可以感到惊讶，但现在没有任何变化！计算机程序仍然以同样的方式工作！Powerfull 笔记本电脑取代了“大铁机”，大量的文件类型被用来代替卡片。但是指令一结束，程序就结束了。你不相信我？只需编写一个简单的控制台应用程序，显示“Hello World！”。

Simple Console Application

你在运行应用程序的时候看到文本了吗？我没有。在我明白发生了什么之前，这个项目就已经结束了。

# 怎么看留言？

你可以说我们需要加上类似`Console.ReadKey()`的东西。但是，你真的认为这种方法可以应用到游戏中吗？我不知道。这个命令完全阻塞我们的程序，直到用户按下任何按钮。一般来说，游戏是一个独立于用户行为的世界。风在吹，水在流，怪物越来越近…

我们怎么能让我们的程序处理所有这些事情呢？答案比你想象的要简单。这几乎是一个无止境的循环，每次迭代都代表游戏直播中的一个瞬间，换句话说，就是用户在屏幕上看到的一个画面。让我们稍微重写一下我们的程序。

Simple Console Application. Loop.

如果你运行这个程序，你什么也看不到。这是因为我们的输出`Console.Write("Hello Word!");`与视频输出不同步(更多细节请看我的计算机图形学文章)。

[](https://antarh.medium.com/computer-graphics-frame-rate-48967ec615d) [## 计算机图形学。帧速率。

### 当你通过代码创建一个动画时，你可能会遇到跳帧或者画面闪烁的情况。这个故事旨在…

antarh.medium.com](https://antarh.medium.com/computer-graphics-frame-rate-48967ec615d) 

我们能把上面的循环称为游戏循环吗？不，我们不能！要称这个循环为“游戏循环”,它应该能够做更多的事情。首先是改变世界。

# 如何改变世界？

一般来说，改变世界语句意味着你需要改变程序状态。在我们的例子中，改变程序状态的最简单的方法是改变组成我们消息的字母的状态。比如说……我们换个他们的案子吧。

Simple Console Application. Change World.

现在我们有了一个由字母代表的世界，它会及时改变字母的大小写。为了把我们的程序变成一个游戏，最后要做的是什么？对！我们需要允许用户与世界互动。

# 用户如何与世界互动？

我认为在我们的程序中，用户可以改变字母大小写的方向。为此，我们需要做到以下几点:

*   添加两个字段:一个保存用户按下的键，另一个存储字母改变方向的状态(0 —右，1 —左)

Simple Console Application. Fields.

*   修改循环以允许我们访问按下的键并处理用户输入

Simple Console Application. User Input.

*   实现`ProcessUserInput();`方法

Simple Console Application. User Input Handler.

*   向`ChangeWorld()`方法应用方向

Simple Console Application. Direction Change.

恭喜你，你刚刚创建了一个游戏！

# 结论。

游戏循环是我们在程序的 main 方法中使用的一个循环。它在程序执行期间持续运行。循环的每一次循环都在没有程序阻塞的情况下处理用户输入，更新程序的状态，并重新绘制用户看到的 word。此外，它跟踪与屏幕重绘同步的回合的执行时间。

感谢您的阅读。我希望你喜欢这个故事。你真诚的，亚历克斯。