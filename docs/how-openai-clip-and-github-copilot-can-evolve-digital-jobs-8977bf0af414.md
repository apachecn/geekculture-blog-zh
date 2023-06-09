# OpenAI Clip 和 Github Copilot 如何进化数字工作？

> 原文：<https://medium.com/geekculture/how-openai-clip-and-github-copilot-can-evolve-digital-jobs-8977bf0af414?source=collection_archive---------38----------------------->

![](img/3d506e1155348e25daec40106457fe36.png)

Photo by [Yancy Min](https://unsplash.com/@yancymin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

想象一下，在一个世界里，所有的数字工作都可以用一种自然语言，一种人类语言来完成。在这个世界里，计算机理解人类需要它做的操作，而且毫不费力。

想象一下，开发人员写下 3 个单词，计算机就能准确猜出是什么。或者更好的是，想象一下，明天，一个开发人员可以用几句话说出他的意图，而机器可以创建这样的应用程序，就像电子商务或移动网络应用程序一样，只需要一点点上下文。

照片编辑监督杂志中出现的图像照片编辑监督杂志中的图像。他们有多年的照片编辑和照片编辑应用的经验。照片编辑是极富想象力的人，他们对细节有敏锐的眼光，收藏令人印象深刻。

想象一下，未来编辑图像的工作可以归结为给出一些声音指令，比如:

——“我想看看她卷发的样子……”

—“好了，现在，自动裁剪照片，设置宽高比以使用水平对齐和与主体的感知距离。”

—“然后应用 90°旋转，应用 3D 透视。好了，搞定”。

这听起来像一个技术乌托邦，但未来就在我们面前。我们未来的工作看起来就像是直接从科幻电影中走出来的一样。

OpenAI 最近发布了两项人工智能技术，将以这种方式补充和扩展人类技能:与微软合作的 Githuh Copilot 和 CLIP(对比语言图像预训练)。

# 利用 Github Copilot 缩小差距

计算机语言旨在弥合自然语言和二进制语言之间的鸿沟。例如，在人类语言中，单词和短语通常有多重含义。另一方面，在计算机语言中，没有歧义，语法正确的命令是精确的。 [GitHub Copilot](https://copilot.github.com/) 是一个 AI pair 程序员，可以帮助你更快地编写那座桥，用最少的上下文做更少的工作。

GitHub Copilot 接受了数十亿行公共代码的训练[。它向您提出的建议适合您的代码，但是代码是由其他人编写的，最终会通知其背后的处理。](https://docs.github.com/en/github/copilot/research-recitation)

首先，我们必须明白，这不仅仅是 Github 中所有信息来源的简单复制粘贴。我们给你的训练不仅仅是这些。这与 [GPT-3](/swlh/gpt-3-and-in-the-beginning-was-the-word-part-1-2-38e67633c315) 的情况相同，系统可以理解信息的上下文。

生成的结果比我们作为输入提供的结果更合理。这也是为什么 Copilot 会参与到源代码中来，了解你想要生成的东西的上下文。例如，它可以是注释中的描述，或者是您所使用的函数的名称，或者是输入变量的名称。有了这些信息，Copilot 就能知道你到底想要什么，并猜测你要求他产生什么。

Github Copilot 可以节省大量的开发时间。此外，Copilot 几乎可以用任何编程语言进行推荐，尽管它与流行的 JavaScript、Python 和 TypeScript 语言配合使用效果最佳。

Github Copilot 基于 Github 的所有库，功能强大。但它并不完美…

一个问题是完整的背景。它理解你当前工作文件的上下文。例如，如果你有一个很长的代码文件，它将理解这个文件内部的变量，并且它可以被其他函数使用(或重用)这些变量。

它就像一个超级强大的自动完成功能，但是它只有您的
文件的上下文，而没有您整个项目的完整上下文，所以它不会理解来自其他文件的代码。IntelliSense 和 VS 代码或者其他软件开发工具一次还是比较强大的。虽然，令人印象深刻。

另一个问题是 Github Copilot 没有遵循最佳实践。它可能不适用于您的代码库所在的版本，并可能导致冲突。副驾驶可能导致[安全问题](https://jakearchibald.com/2021/encoding-data-for-post-requests/)，也可能导致[版本冲突](https://blog.hrithwik.me/the-good-and-the-limitations-of-github-copilot?guid=none&deviceId=e66d1606-567c-4257-8911-899ff5b8994e)。

Github Copilot 创造了一种结对编程方法，在这种方法中，你的搭档不断地给出有问题的代码。

即使它从未达到完美，Copilot 或它的后继者可以完全按照程序员的方式工作；它只会“测试、审查和验证”人工智能代码。我们还会称这些人为“程序员”吗？

# 带剪辑的视觉和语言

OpenAI CLIP(对比语言-图像预训练)是一个从图像和字幕中训练出来的研究工具。

因为 CLIP 的训练集包含概念元数据(文本标签)，所以它可以对以物理、象征或图形方式呈现的概念做出响应。

要理解一个模型作为 OpenAI CLIP 的重要性，我们必须首先专注于理解许多计算机视觉模型的弱点。

我们今天使用的许多模型都非常强大，可以帮助我们解决许多任务。我们还知道，它是发现许多其他技术的先锋，这些技术后来影响了深度学习领域的其他领域。

但这并不意味着这些模型也有很大的缺陷，我们必须了解和分析，以改善它们。我再举一个例子:想象一个卷积神经网络，我们训练它来学习如何很好地分类热狗。但是如果我们想给一个披萨分类呢？

Silicon Valley — Season 4 Episode 4

我们一无所知的网络知道如何对热狗进行分类，它的输出将准备给我们这两个答案中的一个，这就是为什么当我们把一张披萨饼的照片传给它时，它会尽力而为。不过，最终，它会告诉我们，作为一个热狗，你会发现我们可以很好地设计和训练网络来学习和解决某个任务，但一旦它们被训练，我们很难改变它们正在解决的任务。这种缺乏灵活性是众所周知的。

近年来发展的一个最重要的趋势是，在大量不同的图像数据集上使用预先训练好的模型，因此我们知道，初始网络将学习到许多模式，这些模式可用于解决非常不同的任务。

第一个问题是最小程度地修改网络体系结构，以添加负责分类的最后一层。

第二个问题是管理大数据集上的预训练模型，这些数据集是由人标记的图像，这些人必须查看这些图像并决定与这些数据集相关联的标签，这些数据集由数百万手动标记的图像组成。有必要训练越来越大的模型，因为这些数据集也必须越来越大，甚至会出现更深刻的问题。

一种解决方案是 OpenAI CLIP。CLIP 是一个用来解决我们提出的许多问题的提案。这是通过结合两个模型的力量实现的:一个是视觉(分析图像)，一个是语言(处理输入标签)。CLIP 引入了一种替代我们在计算机视觉中传统使用的训练形式。CLIP 必须很好地学习来扩展这个概念，并理解描述和图像内容之间的关联。

这似乎是一个简单的想法，但它有一些重要的含义。Clip 不再需要使用由人仔细标记的图像进行训练，以便其标签准确地表示该图像包含的内容。

现在我们可以用自然语言进行描述。这个人工智能系统必须学会从哪里可以找到大量的图像。

这个人工智能系统必须学习许多图像，用自然语言以不同的方式描述这些图像。CLIP 已经用从互联网上获取的超过 2.5 亿张图片和描述进行了训练。

CLIP 的强大之处不仅在于互联网数据(图片+描述)，还在于它的训练方式，因为 CLIP 了解上下文。毕竟，这个描述是与一个特定图像的内容最匹配的描述，而且，模型知道为什么这些描述不属于原始图像。这个想法是，一个网络不仅要学会找出那个图像和标签之间的关系，还要学会找出与其他图像的区别。什么是众所周知的对比学习

> 对比学习的主要思想是学习表征，使得相似的样本彼此靠近，而不相似的样本相距较远。对比学习可以应用于监督和非监督数据，并已被证明在各种视觉和语言任务中取得良好的性能。— Lilian Weng，OpenAI 的人工智能研究经理

这个模型是引人注目的，因为它实际上为我们提供了许多令人兴奋的工具。但是，毕竟，首先我们拥有的是一种技术，它已经很好地学会了将图像与其内容描述结合起来的任务，这种工具最终达到了拥有一个真正的语义搜索引擎的想法，它不仅可以理解我们引入搜索引擎的标签和元数据，还可以理解内容描述或来自图像的内容。

这是一种新的训练模式。无需修改预训练模型架构中的最后一层。不再需要重新训练网络来对热狗和披萨进行分类。不再需要成为深度学习工程师并理解我们正在使用的技术，而是任何用户都可以创建自己的分类器。视觉与自然语言的融合突然将我们带到了[语义网](https://daniel-leivas.medium.com/the-specter-of-the-semantic-web-3f85addbaf18)。这很酷。

你可以试试这里的。

# 最后的想法

那些 AI 并不是要摧毁任何职业，AI 工具是帮手。

显然，Github Copilot 可以破坏数小时的编程工作。这些人工智能的目标是生产力。作为副驾驶的工具可以减少编程劳动力市场中的许多工作时间。但是关于这种人工智能的进化还有许多问题，我们可以与人工智能建立共生关系。

如何估计未来几年劳动力市场的发展趋势？发展中的事情会有怎样的变化？这些人工智能工具的进化已经复杂得多了。我们将不再知道这种影响是消极的还是积极的。

人工智能发展的步伐似乎可能会继续下去。我们可以期待看到令我们震惊的常规突破。这样我们就直接用自然语言指令计算机了。将鼓励新的公众进入劳动力市场。

# 摘要

*   OpenAI 最近发布了 CLIP 和 Copilot 两项 AI 技术，将补充和扩展人类的技能。
*   即使它从未达到完美，Copilot 或它的继任者也能完全掌握程序员的工作方式。
*   CLIP(对比语言-图像预训练)是一种从图像和字幕中训练出来的研究工具。CLIP 结合了两种模型的力量:一种是视觉模型(分析图像)，另一种是语言模型(处理输入标签)。
*   作为副驾驶的工具可以减少编程劳动力市场中的许多工作时间。但是关于这种人工智能的进化还有许多问题，我们可以与人工智能建立共生关系。
*   人工智能发展的步伐似乎可能会继续下去。因此，我们可以期待看到令我们震惊的常规突破。这样我们就直接用自然语言指令计算机了。

感谢阅读。

请在 Medium 中关注我，这样您就不会错过接下来的文章。

[](https://copilot.github.com/) [## GitHub Copilot 你的 AI 对程序员

### 使用 GitHub Copilot，您可以在编辑器中获得整行或全部功能的建议。数十亿美元的训练…

copilot.github.com](https://copilot.github.com/) [](https://docs.github.com/en/github/copilot/research-recitation) [## 研究朗诵

### 作者:Albert Ziegler (@wunderalbert)首先看看 GitHub Copilot 建议中的死记硬背。GitHub Copilot 是…

docs.github.com](https://docs.github.com/en/github/copilot/research-recitation) [](https://www.cnbc.com/2021/06/29/microsoft-github-copilot-ai-offers-coding-suggestions.html) [## 微软和 OpenAI 有了一个新的人工智能工具，可以给软件开发者提供编码建议

### 几十年来，研究人员一直试图让程序编写程序。微软和 OpenAI 正在利用巨大的云…

www.cnbc.com](https://www.cnbc.com/2021/06/29/microsoft-github-copilot-ai-offers-coding-suggestions.html) [](/swlh/gpt-3-and-in-the-beginning-was-the-word-part-1-2-38e67633c315) [## GPT-3:最初是这个词(第 1/2 部分)

### 30 秒总结

medium.com](/swlh/gpt-3-and-in-the-beginning-was-the-word-part-1-2-38e67633c315) [](https://blog.hrithwik.me/the-good-and-the-limitations-of-github-copilot) [## Github Copilot 的局限性和优点

### 几个月来，我一直在尝试用 GPT3 生成代码，最近获得了一个更好的…

blog.hrithwik.me](https://blog.hrithwik.me/the-good-and-the-limitations-of-github-copilot) [](https://jakearchibald.com/2021/encoding-data-for-post-requests/) [## 为 POST 请求编码数据

### 现在，当你去 copilot.github.com 时，你会听到这样的例子:这是不好的，可能会导致安全问题…

jakearchibald.com](https://jakearchibald.com/2021/encoding-data-for-post-requests/) [](https://openai.com/blog/clip/) [## 剪辑:连接文本和图像

### 我们正在引入一个叫做 CLIP 的神经网络，它可以有效地从自然语言中学习视觉概念…

openai.com](https://openai.com/blog/clip/)  [## 对比表征学习

### 对比学习的主要思想是学习表征，使相似的样本彼此靠近…

lilianweng.github.io](https://lilianweng.github.io/lil-log/2021/05/31/contrastive-representation-learning.html) [](https://daniel-leivas.medium.com/the-specter-of-the-semantic-web-3f85addbaf18) [## 语义网的幽灵

### 人工智能(AI)技术正日益融入我们的生活。仍然存在一个…

daniel-leivas.medium.com](https://daniel-leivas.medium.com/the-specter-of-the-semantic-web-3f85addbaf18) [](https://clip.backprop.co/) [## 剪辑演示|背面投影

### Backprop 上托管的 Open AI 的 CLIP 模型演示。

clip.backprop.co](https://clip.backprop.co/)