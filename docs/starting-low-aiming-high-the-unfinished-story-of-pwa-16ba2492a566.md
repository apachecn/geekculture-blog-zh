# 起点低，目标高:PWA 未完成的故事

> 原文：<https://medium.com/geekculture/starting-low-aiming-high-the-unfinished-story-of-pwa-16ba2492a566?source=collection_archive---------53----------------------->

![](img/3f39aead12a37dcdaeb88a715d20034a.png)

Photo by [Carl Heyerdahl](https://unsplash.com/@carlheyerdahl) on [Unsplash](https://unsplash.com/photos/KE0nC8-58MQ)

渐进式 web 应用程序(PWA)在其发展过程中经历了过山车般的起伏、回卷和转弯。问题是——它将把他们带到哪里，我们对他们有什么期望。

PWA 的想法从一开始就很棒。谷歌将本地应用的功能和网络的可访问性结合起来，做得非常出色。

根据维基百科，PWA 是一种通过网络交付的应用软件，使用网络技术构建。它融合了网络和本地应用的优势，旨在在任何平台、任何设备上工作，不管你使用的是什么:智能手机还是电脑。

公共福利协会有三大支柱:

1.  能干。PWAs 功能接近原生应用。您可以创建具有多种功能的 web 应用程序，包括推送通知、地理定位、视频、音频等。
2.  可靠— PWA 速度快，可以脱机工作。
3.  可安装— PWA 可以像本机应用程序一样安装。

谷歌在 2015 年推出了 PWA(我们自己的想法始于 2007 年)。一些大公司(Twitter、Pinterest、星巴克、Spotify、优步和福布斯)决定转向 PWA，并宣布了用户参与度的良好结果。分析显示，Twitter 增加了用户数量，显著增加了每次会话的页面数，速度提高了 30%。Twitter 是 2018 年首批转向 PWA 的公司之一。2021 年初，Twitter 宣布他们的 PWA 在 Windows store 中。福布斯显示，读者数量增加，文章平均阅读时间和滚动深度增加。所获得的结果使我们可以断言 PWA 是一个优秀的商业解决方案，尽管多年来 PWA 一直与一些隐私问题有关(苹果在 Safari 中阻止了几个 PWA 功能)。

与 PWA 同年，另一项名为 AMP(加速移动页面)的技术组件问世。它的创建是为了让网页加载更快。虽然 PWA 是 web 技术的一个真正突破，但 AMP 只是一个具有重大矛盾影响的小东西。

一切看起来都很好，直到 2021 年，宣布了两个重要而出乎意料的决定:

1.  AMP [从 2021 年 6 月](https://plausible.io/blog/google-amp?ref=webdesignernews.com)起不再供货。
2.  旨在解决安全和数据保护问题的 PWA 离线支持检测的改进[搁置](https://developer.chrome.com/blog/improved-pwa-offline-detection/)。

值得注意的是，这两个决定都是在开发者社区的强烈影响下做出的。

AMP 的主要好处是它在谷歌的搜索结果中享有优惠待遇。在推出 AMP 之后，有很多关于谷歌试图征服网络或者创造自己的网络谷歌化版本的讨论。哈佛大学内曼新闻实验室主任约书亚·本顿(Joshua Benton)[说](https://www.politico.eu/article/google-amp-accelerated-mobile-pages-competition-antitrust-margrethe-vestager-mobile-android/)，我们正在从一个你可以把任何东西放在你的网站上的世界向一个你不能这样做的世界转变，因为谷歌这么说。自由是本质，但让 AMP 消亡的真正原因是开发者在不使用 AMP 的情况下编写的 web 应用比 AMP 运行得更快[。](https://plausible.io/blog/google-amp?ref=webdesignernews.com)

凯文·巴塞特给出了一个很好的解释,解释了为什么谷歌在 PWA 的改进上后退。PWA 的关键问题是关于离线和安装的强烈要求，而没有任何使开发者的生活更容易的提议。

> 考虑到缓存是如此脆弱和困难，完美的实现对于大多数开发人员来说可能是遥不可及的。通过破坏其他东西来迫使他们就范可能不是办法。

最近围绕 AMP 和 PWA 发生的所有事件让我觉得使用这些技术的开发者可能会不满意。作为一名开发人员，我得出了以下关于 PWA/AMP 方法的错误之处的结论:

1.  **PWA 并不是一个涵盖 web 应用程序所有方面并使开发人员生活更轻松的完整概念。** Google 认为 PWA 只是运行在客户端(浏览器)的代码，完全忘记了 web 应用可能有接口和服务器端的部分。假设我们想要创建一个 web 应用程序，它具有吸引人的动画界面和支持 RESTful 和 CORS 的服务器端代码。在这种情况下，PWA 不是一个解决方案，而是另一个“必须”需求。
2.  **我们对本地和网络应用使用完全不同的开发技术。**使用“应用”这个词，我们希望使用相同的开发技术和模式，包括单一的编程语言和将我们的产品从一个平台移植到另一个平台的能力。PWA 在这种情况下也不是一个解决方案。我们仍然使用复杂的语言、框架、工具来开发 web 应用程序，比如 web 页面，将自己分为“前端”、“后端”和“全栈”开发人员。

我认为 PWA 和 AMP 的混乱走势是上述基本矛盾的结果。PWA 是一个很好的想法，但是开发人员希望这个解决方案能够使 web 应用程序的创建更容易、更可靠、更接近本地应用程序。