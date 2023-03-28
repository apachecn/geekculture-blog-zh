# 你能向左移动多远？

> 原文：<https://medium.com/geekculture/how-left-can-you-shift-aedf42d73f78?source=collection_archive---------20----------------------->

![](img/2f3de7722a85fbff502473b7c90f9b79.png)

Photo by [Giulia May](https://unsplash.com/@giuliamay?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/left?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

亲爱的读者，我不确定你对这篇文章有什么想法，但我想直接让你知道，左移与福利国家无关。至少这篇文章里没有。

抛开这些不谈，左移在软件开发中意味着什么？

Kirstie Magowan 在她的博客中相当优雅地定义了它:

> 左移是一种旨在软件交付过程早期发现和防止缺陷的实践。

虽然这已经基本确定了，但我想稍微扩展一下定义，说**左移是为了更快地获得对你工作的反馈**。不仅仅是缺陷的问题。

那么我们如何做到这一点呢？

让我们看看我们可以应用这种实践的几个领域。

# 代码审查

![](img/3791cb3b54cf7b5cca5cfbd0345d93e6.png)

Photo by [Alvaro Reyes](https://unsplash.com/@alvarordesign?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/code-review?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

长期以来，我一直是代码审查的积极支持者。我坚持认为，您应该对源代码控制进行小的修改，并在进行过程中对它们进行评审。

因此，这将代码审查向左移动。但是我们可以做得更好。好多了。

尽管代码审查过程非常有价值，但它不是一个非常高效的过程。当面对大量代码时，[我们倾向于浏览它](https://smartbear.com/learn/code-review/best-practices-for-peer-code-review/)，而不是回顾它，正如 Giray ozil 所说:

> 让一个程序员检查 10 行代码，他会发现 10 个问题。让他做 500 行，他会说好看。

对话可能是这样的:

*技术主管:“那么……这有用吗？”*

*开发者:“对。”*

*技术主管:“嗯……我想没问题。我们稍后会修复它。”*

不你不会的。让我们现在修理它。

怎么会？尝试**配对编程**。

你可以和你的队友轮流，得到业务逻辑的即时反馈，错误检查(最好在别人的棋盘上完成，不是吗？)、代码风格甚至 IDE 使用——天知道我是通过看别人打字学到了最好的 IDE 窍门。

*“等等，等等……你刚才做的事——你是怎么做到的？”*

我甚至建议把它变成强制性的。您的组织将因此受益，并且您会发现大多数开发人员都喜欢这种体验。尤其是现在我们很多人都在远程工作，人性化的效果是不可估量的。

# 质量

你是否曾经意识到你的软件有太多的特性需要手工测试？我发现自己在这种情况下的次数比我想承认的要多。

事实证明，面对越来越复杂的软件，庞大的人工回归套件并不是一个好的质量控制措施。它们是不可持续的。见鬼，他们甚至不可靠！谁真的想经历成千上万的步骤来检查每一个角落和缝隙，以确保软件仍然做它应该做的？

所以，是的，我们必须编写自动化测试。但是是哪些呢？

谷歌测试金字塔说明了一切:写得很少，非常关键(但是非常健壮！)端到端测试、一些集成测试和大量的单元测试。多少？分别为 10%、20%和 70%。

好棒的单元测试！

所以只要写一大堆单元测试——就完事了，对吗？嗯……也许吧。

最近我一直在想，这些年我们可能都做错了 QA。也许测试是 QA 工程师工作中最不重要的部分。但这是另一篇文章的主题。

# 构建和部署

![](img/d8fc844ebd755d707ac9e3505bc76231.png)

Photo by [Randy Fath](https://unsplash.com/@randyfath?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/build?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

你或你的开发人员偶尔会合并一些破坏构建的东西吗？

当然，我们都应该在合并之前构建自己的代码，亲爱的读者，我相信你也是这样做的。然而，我有时会因为匆忙而忘记了我的尽职调查，我怀疑还有一些人和我一样。

比起依赖开发人员的纪律，我更喜欢自动化的预合并管道。这样，无论是谁在查看 pull 请求，都将能够看到代码在合并之前破坏了构建。预合并管道应该使用与生产版本相同的生成脚本和配置，并且应该签入源代码管理。

类似地，部署脚本和配置也应该在源代码控制中，并用于您部署到的每个环境。

要完成这三项任务，请将基础结构配置检查到源代码控制中，并根据该配置实例化资源。这样，环境供应也变得可测试和可预测。

# 安全性

![](img/63c57a7ab3f9b58d0ba89aae770b5be6.png)

Photo by [Matthew Henry](https://unsplash.com/@matthewhenry?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/security?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我认为有三种简单的方法可以让安全性左移:规划、教育和自动化测试。

## 规划

从一开始就让您的安全专家参与进来。告诉他们你计划为谁构建什么，你的系统可能会有哪些组件和接口。作为回报，他们将能够尽早告诉您潜在的漏洞和数据保护问题，以便您可以将这些融入您的用户故事和架构中。

## 教育

[安全风险](https://owasp.org/www-project-top-ten/)和[数据保护](/ft-product-technology/a-developers-guide-to-gdpr-that-won-t-make-you-sweat-4f1f7f1d9c8b)应该是每个开发人员持续学习的一个组成部分。开发人员与这些主题的互动越多，它就越成为他们思考的自然部分。应该为课程和培训留出时间。

另一种教授安全性的方法是渗透测试培训。在我们自己的软件上进行培训时，我参与度很高。试图破坏你自己建造的东西是一种奇怪的乐趣。

## 自动化测试

最后，当代码进入时，您应该运行自动化的安全性测试。这些可以由您的开发、QA 或安全团队定制，或者由第三方工具提供，如 [OWASP ZAP](https://www.zaproxy.org/getting-started/) 。

# 你能向左移动多远？

正如你所看到的，左移不仅仅是检测缺陷，尽管它是实践的焦点。这是关于不断学习如何从一开始就做得更好，直到你可以[第一次就做对为止](https://kanbanize.com/continuous-flow/jidoka)。

如果你还没有完成其中的大部分，你可能会想:这是一大堆东西，我们应该从哪里开始呢？

我的推荐？从结对编程开始。我不能夸大这种做法的价值。你将团队建设、学习、代码审查和质量工作都打包在一起。我保证，你不会后悔的。

接下来，一个好的 CI/CD 渠道会大有帮助。让自动化检查和测试发挥作用，并宣布绿色构建策略。这将避免许多多余的工作。

剩下的就看你的了。所有这些都很重要，都有回报，但有些不太容易实现，在一定程度上取决于你的团队和公司结构。

我祝你旅途好运！

保持冷静，向左转。