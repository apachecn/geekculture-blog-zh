# 用错误的方法获得正确的工具(Java 代码，EMA-交叉股票分析)

> 原文：<https://medium.com/geekculture/the-wrong-way-to-get-the-right-tool-java-code-ema-crossover-stock-analysis-99a1b38a7301?source=collection_archive---------20----------------------->

关于为什么使用 EMA(指数移动平均线)价格交叉分析，有几个吸引人的论点。结合每天发生的交易量信息，我们可以更仔细地观察价格的变化，并解读交易者的行为。也就是说，如果我们在交易量相对较大的一天看到价格大幅下跌，我们就知道大部分股票被卖出了(请原谅我的简单化)。这使得分析师能够将因果关系与那些时间段内发生的重大事件联系起来。

出于这些原因，我的一个朋友想要一个简单的均线-均线-价格交叉分析工具。

我的做法完全错误。这是一个警告性的故事。

这是在我知道。“csv”文件代表“逗号分隔值”之前我知道它们很容易下载和使用。在我了解这个世界之前，看起来。我只知道 Java。就好像我住在一个没有互联网连接的壁橱里，只有一台装有 NetBeans 的计算机。

让我澄清一下:我做这件事并不容易。而不是去雅虎财经网站下载。一段时间内历史价格数据的 csv 文件，我(停下来喘口气)使用用户的输入呼号和时间戳生成器建立 Yahoo Finance 历史数据网站的 URL，连接到该网站以接收您的浏览器通常会构建到网页中的 HTML 转储，并通过在标记处分割 HTML 来系统地收集所有价格信息，我注意到这些标记彼此相隔几天。这样我就可以建立一个窗口，每天垂直分段，价格数据和两个均线指标穿过它们。

这些行的顺序很重要。为了突出显示排序，我将窗口上日期段的背景色改为该排序的唯一颜色。这需要 6 种颜色来做(每种顺序 3 行)。另外，我根据均线和价格线的相对距离来调整颜色的强度。也就是说，绿色越浓，表明价格相对高于短均线和长均线。我保留了图表的底部作为音量指示。当天底部的金条越亮，当天交易的股票越多。这种颜色就是我如何让这个系统“一目了然”的。然而，在这个结构中，如果需要更多的数据，您只需将鼠标悬停在一天的某个时间段上，就会弹出一个工具提示，其中包含关于这一天的更深入的数据。事实上，它看起来有点像这样:

![](img/ef09738ff26e20b180379a82e836f853.png)

我的一个朋友向我解释说，要求使用这个工具的人可以从类似于“买灰色卖蓝色”的图表或一些有意义的组合中推断出“低买高卖”的规则。

我在这里为您提供了在没有主要项目导入的情况下制作自己的工具的能力。事实上，它和 Java 一样具有可移植性，除了活跃的互联网连接之外，它没有外部依赖性。这可以为您节省大量手动获取数据的时间。然而，我采用的方法也有一个有趣的缺点。

为了看到这个缺点，我向你提供了在网页上滚动的概念。通常，或者至少在 Yahoo Finance 上，当你设置历史价格数据并点击“应用”时，并不是所有的数据都会立即传送到你的浏览器中。事实上，只有一小部分是。你必须向下滚动网页，它才能继续加载更多的价格到你的本地机器上。这意味着当这个程序连接到 Yahoo Finance 时，它通过 HTML 接收的价格数据的天数有一个绝对上限。我通过基于丢失的时间框架重新构建 URL 来解决这个问题，但是在继续这个项目之前遇到了日索引的问题。然而，我在用户用来构建项目的第一个选项框中使用了一个巧妙的技巧，提供了一种有效的方法来充分利用单个连接。给定有限的天数，较大的 EMA 窗口会限制实际拥有 EMA 数据的天数。较小的窗口需要较少的初始化时间，可以提供更多的 EMA 数据。因此，每次用户选择特定的 EMA 窗口大小时，我都会更新用户可以查询的可用天数列表。如果用户选择要显示的天数，我会更新可用 EMA 窗口的列表，允许显示这些天数。选择一个决定另一个的可用性。要看到更多的日子，你必须使用更小的 EMA 窗口。但是要注意，最终的显示中总会出现一种 8 日均线。这是运行程序时选项对话框的样子(窗口示例类似于用于构建上图所示窗口的示例):

![](img/37ddb0ccdaa59741d3dc7374367b92aa.png)

我认为在这里展示的代码不仅仅是一个示例。我认为这就像展示一幅画。油漆房子没有一种方法。甚至不止一栋房子。我保证。这里有很多东西。去学习。鄙视。简单来说，就是看实际情况。如果你实现了这样的事情，我保证你会有一种成就感和鼓励感。这幅画最酷的地方在于你可以复制它，并与之互动。只要小心行事，作为年轻的作者，我从来没有预料到会有人读它，而且它的某些部分看起来命令密集。

出于这个原因，我请求你原谅代码中任何带有咸味的措辞，因为我不觉得有必要仅仅为了保护观众不受这些话的影响而重新绘制这幅明显的画。咸词是 Linux 内核臭名昭著的东西。或许我们可以称之为灵感，如果不是相似的话。我提供代码修剪，但在其他方面是一样的。我只是绒毛出一些不使用的组件，真正为照片做好准备。我做完后还会把它们放回去。我也不会花时间解释我在 Java 中的每一步。这篇论文太长了，我们双方都不会感兴趣。只需知道，这确实使用了一些更先进的技术，在一些适度的水平。

从项目的核心到枝蔓，我将从驱动文件开始。相信我会继续在这个项目空间，我写了一个通用应用程序启动器，这个股票应用程序是它启动的唯一应用程序。毕竟，这是一个如何以错误的方式做事的故事，尽管可行。

![](img/6bcf018c481ab29ea2aa08798e74ae18.png)![](img/03eafd41ca438d9ed04c1be24d831f71.png)![](img/086f93c52610b5d59e7673d527dc32a4.png)![](img/a285668a42207950ab0ab23910662d95.png)![](img/efe2dda79dfd40402ebb199ecfdf339a.png)![](img/cf54ace3f1a6dfad77faaf175097982f.png)

显而易见(半开玩笑地说),编译的主要要求是存在一个 StockCaller 类，如第 89 行所示。在我展示 StockCaller 类之前，我想谈谈它的最后一行。没有任何理由。其实我都不用。有趣的是，它被认为是那种你可以看着它叹息的东西。’。用 python 编写的 Java 等价物:

将 numpy 作为 numberbasedpythonlibrary 导入。

我的愚蠢如下:

类 WindowPane 扩展了 JFrame { window pane(){ super()；}}

这应该让你问这个问题'*为什么？*'

又来了。代码是按原样提供的，这是对如何不去做事情的深入研究。

至于股票经纪人..

![](img/823259f8a51d01463657ab618e7159df.png)

从这里，我们看到对三件事情的依赖，在第 19 行初始化的数据池，在第 27 行初始化的 StockView，和在第 38 行初始化的 StockMath。

数据池类是运行在后台线程上的 StockMath 和运行在事件调度线程(EDT)上的 StockView 之间的中介。StockMath 获得所需的数据，将其存储在同步队列中，StockView 从那里挑选出数据来构建窗口。Java 中的同步可能很棘手。但我向你保证，接下来的很可能是最好的错误方式。

从三者中最基本的功能开始，我提供数据池。

![](img/42bddf44dcd705abd49ce3121a818f86.png)![](img/657ee10de4fb5f17f31322d73d268f0f.png)![](img/440d291da56fc54eeda0ee775c2bc411.png)![](img/871889e93d0896d58f6915f56a627b2b.png)

尽管文件很长，但我们看不出这里有什么大的变化。我们只是需要一个地方来临时存储一些数据。

我想在这里澄清一下，我的目标不是攻击我的作品，并试图把它揭露成糟糕的东西。它很快很好地完成了工作，几乎不需要用户的输入。当我回顾我完成这件事的方式时，我忍不住笑了。我现在更看重效率，而不仅仅是过程。我过去只是把它做完，现在我喜欢认为我做得很好。然而，这个交叉项目仍然是一个有用的工具。如果一个人将金属制成锤子，而另一个人用手塑造它，他们展示了两种不同的方法来获得独特但相似且有用的工具。原来我用一大块铁和一个指甲锉做了这个特殊的锤子。

继续进入 StockMath:

![](img/44178851a3c433203a23380cf9fc4821.png)![](img/2a9eb95a5581ebf355677c7bbef48a91.png)![](img/ef9c1bd0d6e5c1bc252c84384323d38c.png)

这首先需要 URLGenerator，如第 50 行所示。这个自定义类是我如何从用户输入的呼号中构建 URL 的。比方说，我想检查先进的微型设备的股票。作为用户，我在第一个窗口中输入“AMD ”,然后 StockMath 使用 URLGenerator 通过 Yahoo Finance 的网站开发 AMD 历史数据的 URL。

![](img/12a75b20371c2fb3ce3589b2d8bf9d0c.png)

StockMath 然后与一个名为 HTMLParser 的依赖项进行交互，如第 52 行所示。这不是真正的 HTML 解析器，而是我编写的一个类，用来解析从 Yahoo Finance 返回的 HTML。它有两个功能:挖掘结算数据和挖掘体数据。然后 StockMath 类根据收盘数据计算 EMA 值。在我展示 StockView 之前，让我用 HTMLParser 来讨好你。

![](img/4a3331208ca9aa84a077ac2a8a8933f0.png)![](img/2df19006f6de92b28912567ac1105474.png)![](img/ad4ea3315aeb0f426741185ebb7ba3ce.png)![](img/1cd70bf0a17de8c20e3a09d94ea02eb5.png)![](img/8a8bd2772e91d09579f0fbbcc46328c9.png)![](img/2964776ab2e557a214274675982ba51a.png)![](img/bcbdc7880566a58b9d9d6ea30d5ec3e0.png)![](img/fb70636fd1d742c282662bff59f83a8c.png)![](img/ce9ce91786f623e2271ec991a25703a2.png)![](img/1175a9ffd723924506885229f1e9f3a1.png)

太好了！现在我们有了最后一个依赖；斯托克威尔。我觉得有必要在这里提醒您，Java 为定制提供了很多选择。这意味着只需几个指令就能把所有事情都安排妥当。正因为如此，StockView 是这个项目中最长的班级群。虽然视觉上很简单，但程序显示的窗口可能需要相当多的描述才能正常工作。作为证据；

![](img/624b7ffe756353bb6050193c2ea9b3ab.png)![](img/a9f1b993c1875688f861ba53f22a54ce.png)![](img/d5ab6bae32b432fc4131720e50e09602.png)![](img/c44d0298bd7412e405dd89f1628c5a1f.png)![](img/321171a339152b7046df3622cf261c1e.png)![](img/706da2bf2bf8501fb5e6f9d5e8a24839.png)![](img/92ccea29d0934ac68f4de39ce57f2b15.png)![](img/1e1a5524e9ae50681d8c149294a9b316.png)![](img/79a6295933c36e15043c458dd50bf087.png)![](img/d2c61919ed7d28ef4251a31026b2e5a0.png)![](img/8d1bf841005744074455ba708f5ce784.png)![](img/1e92cd869b1d987d2a7258d1e6f8d2b5.png)![](img/d5ee93576951e8f16f88e8b451eaf24a.png)![](img/d47187ad5e342ec9ef32ade322ad03cc.png)![](img/8273bc47277164c02333864354e988da.png)![](img/59a21f3550e36188d06b13fc5f8e1c61.png)![](img/d21b0b159b7294e8405f11ed91642a4b.png)

这个项目到此结束。我认为它是很好的中级水平练习和范例，并且我以它作为我自己的画而自豪。然而，我不鼓励不要利用已经存在的工具。奇怪的是，这个项目只花费了一两次。因此，我个人并没有为此牺牲太多，而是让它变得更“轻松”,来消化我在写它的时候是多么地偏离自己的方式。

长话短说，似乎没有理由不去实践你热爱的艺术。你可以用不同于任何人的方式来创作这幅画，不要让他们告诉你你不适合这幅画。正如你可以用英语发展你的写作风格一样，你也可以用任何计算机语言发展。这些只是完成事情的简单方法。我对你们的探索致以最良好的祝愿，对你们对有趣的事物感兴趣的知识感到欣慰。