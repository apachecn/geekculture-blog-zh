# 榆树 vs 反应——简短的想法

> 原文：<https://medium.com/geekculture/elm-vs-react-brief-thoughts-7f1f3fbbff10?source=collection_archive---------6----------------------->

## 有经验的 React 开发人员如何看待 Elm

![](img/ab31d6414d29648564706f425753cf81.png)

Photo by [Sigmund](https://unsplash.com/@sigmund) on [Unsplash](https://unsplash.com/photos/jbwFv4chusE)

我已经在榆树附近玩了将近 4 个月了。现在我对这项技术有了一些想法，可能对那些正在考虑是否选择 Elm 来开发项目的人有用。简而言之，我现在可以说，自从我开始探索榆树世界以来，我的观点发生了很大的变化。

# 第一个月

在第一个月，我真的对此感到兴奋，并认为这对我未来的项目可能是一件好事，并且很可能我在某个时候建议它用于商业应用程序开发。我花了一些时间阅读文档并探索基础知识。一切看起来都清晰可爱。但是后来我开始用 Elm 开发我的个人博客…

就在那时，我下定决心要用 Elm 开发我的个人博客。做出这一决定有一些原因:

*   我从未体验过的全新技术。
*   Elm 被称为“潮人科技”，它肯定会提高我在队友中的评价。(是啊，挺傻的理由。但我想说实话。)
*   它建立在 FP 理念的基础上，上次我对 FP 非常感兴趣。
*   我喜欢奇怪的解决方案，所以决定满足我的自我。

所以…迈出第一步花了我比我最初预计的多得多的时间。其实一个晚上启动一个 app，并没有明确的教程。Elm 不是关于快速和廉价的解决方案，而是关于享受函数式编程的过程。就因为我有空闲时间，没有截止日期，我慢慢地走着自己的路，并与许多意想不到的问题作斗争。

拜托，想想榆树至于享受与一大堆意料之外的问题斗争的过程！迈出第一步时，你会面临许多问题:

*   路由(URL 解析器，根据全局模型更新视图)。
*   从后端获取数据。
*   HTML 和 CSS 和你已经知道的很不一样！

# 第三个月

这是惯例月。在这一个月里，我取得了很大的进步。结果甚至可以在网上看到:我已经把我的博客部署到 GitHub 页面上了。我仍然有一些 GitHub 动作的小问题，但主要的是从这一点上我能够在我自己的网站上发布我的帖子，而不是在媒体上！这对我非常重要。

# 第 4 个月

我来了。写这篇新博文到我用 Elm 开发的个人博客。其实我对未来还有很多规划。但是现在我有权将我在 Elm 的短暂经历中的一些有价值的产出正式化。希望这能让你对这种奇异的技术有一个模糊的认识。

# 学习曲线

我是一名经验丰富的开发人员，主要从事主流工作:PHP (Symphony，Yii)，JavaScript (React，TypeScript，Flow)，Python，也许还有其他的。由于这个原因，我认为我可以从 Elm 开始，既快又容易。这是一个大错误！即使有使用 Haskell 的小经验(只是为了好玩)，从 Elm 开始也是相当困难的。不是因为语法或其他什么——而是因为缺乏端到端的教程和文档。

所以。做好奋斗一两个月的准备，然后你会开始做一些有价值的事情，这些事情可能会呈现给人们。重要的是——不要选择这项技术来做商业应用，因为你可能会让你的公司破产！只有当你有一个经验丰富的 Elm 团队时，你才可以选择它作为商业应用。

然后考虑在业务增长的情况下招聘。我相信从市场上直接雇佣一个人来开发榆树几乎是不可能的！据我所知，一些关于闭包脚本的例子，公司雇佣工程师并教授他们。而且学习曲线也不是那么容易。对我来说，这是一种非常昂贵的方式。

# 发展速度

Elm 的开发速度非常慢。我这么说是从我为自己开发应用的角度出发的。现在想象一下，你需要领导一个团队使用这种奇特的技术开发一个商业应用。这可能会导致真正的软件开发地狱！

另一方面，我认为建立组件驱动的开发是可能的——这是当你使用已经存在的组件构建新特性的时候。(想想 React world 的故事书。)对于像博客这样的简单应用来说，这可能是相当快速和容易的。但不确定这对 UI 密集型商业应用是否方便。

# 安全

Elm 在一个真正类型安全的应用程序中输出。您永远不会遇到 JavaScript 中常见的“未捕获的类型错误:无法读取未定义的属性”错误。这是 Elm 的一大优势。另一方面，这种安全性意味着您需要编写更复杂的代码。我认为使用 TypeScript 开发类型安全的应用程序是可能的(如果遵循一些 FP 原则，如[“总体性”](https://kowainik.github.io/posts/totality))。)

# 摘要

综上所述，我可以说榆树不是一件容易的事情。它允许你制作一个可靠的应用程序，但你需要为此付费。

我计划将 Elm 用于我自己的目的。例如，我想使用 Elm 继续我的博客开发。但是，商业应用呢——我仍然倾向于主流技术。比如 React 并不是因为是脸书发明的才这么受欢迎；它之所以被广泛使用，是因为它相当简单，围绕它有很多工具和解决方案。想想 Relay——一种更酷的脸书技术，但由于其复杂性和糟糕的社区而不受欢迎。说到 Elm(或者类似的东西)——你需要自己开发很多东西。

但是，例如，将 Elm 用于学习目的仍然是一个好主意。它是一种函数式语言，你可以从中学习到很多有用的概念和方法。Elm 中的许多东西可能会在不同的语言中使用。如“totality”在 Elm 中必须使用(编译器强制)，在 JavaScript/TypeScript 中可能使用，使代码更可靠。

我可以详细阐述整本书的 FP 主题。但是我们暂时就此打住。让我给你一些有用的链接，让你更深入地了解 FP 和 Elm。

*   [官方榆树文档](https://elm-lang.org/docs)
*   [非官方榆树指南](https://elmprogramming.com/)
*   [弗里斯比教授的 FP 指南](https://mostly-adequate.gitbook.io/mostly-adequate-guide/)
*   [科瓦尼克(关于哈斯克尔)](https://kowainik.github.io/)
*   [哈斯克尔的书](https://haskellbook.com/)
*   [又一本哈斯克尔的书](http://learnyouahaskell.com/)
*   [我第一次尝试用 JavaScript 写 FP](https://balovbohdan.github.io/#/post/fp-notes-purity-1)

[这里](https://balovbohdan.github.io/#/post/elm-vs-react-brief-thoughts)你可以在我的个人博客里找到这篇文章。不要把它判断得太强，它处于非常初级的阶段。