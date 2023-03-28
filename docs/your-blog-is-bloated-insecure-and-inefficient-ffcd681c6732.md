# 你的博客臃肿、不安全、低效

> 原文：<https://medium.com/geekculture/your-blog-is-bloated-insecure-and-inefficient-ffcd681c6732?source=collection_archive---------11----------------------->

## 为什么大多数人应该远离 WordPress 网站？

![](img/8c6d9e600d6f7330e058c63388a91516.png)

Photo by [Souvik Banerjee](https://unsplash.com/@rswebsols?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

当你输入的时候

> 如何开博客？

在搜索引擎中，你会得到很多建议，告诉你去创建一个 WordPress 网站。所有的插件和一键安装意味着创建这些博客很容易。截至 4 月 5 日， [WordPress 网站占所有网站的 42.5%以上](https://w3techs.com/technologies/details/cm-wordpress)。然而，你会知道什么是

```
Popular != Good
```

因为 WordPress 不仅臃肿低效。不仅如此，它们对普通人来说也是一个安全隐患。

这就是为什么你的博客内容管理系统臃肿，不安全，效率低下。

## WordPress 网站很慢。

WordPress 动态创建网页。动态网页即时创建网页。通常，这些网页上的内容是从数据库中获取的。随着你添加更多的主题和插件，数据库中的内容也会增加。因为这些[扩展没有被直接烘焙到原始代码库中](/geekculture/your-linux-terminal-is-bloated-12e5b83a1180)。这导致网站运行速度变慢。这种放缓不仅影响消费内容的用户。但也包括创造内容的制作者。不仅如此，增加的扩展增加了安装恶意软件的可能性。这就把我们带到了下一点。

## WordPress 网站是一个安全隐患。

最近[超过 90 个 WordPress 主题和插件被植入了后门](https://www.bleepingcomputer.com/news/security/over-90-wordpress-themes-plugins-backdoored-in-supply-chain-attack/)。360，000 个活跃网站在这些供应链攻击中受损。其中一个网站可能在系统中有你的电子邮件地址。这就是为什么你[应该给一个电子邮件别名，而不是你使用](/geekculture/protect-yourself-with-email-alias-f10ce787cae)的任何服务的主要电子邮件地址。这些内容管理系统本身并不存在安全风险。那些使用最佳实践的人最不可能受到损害。正是那些通常不遵循最佳实践的人最终使他们的网站受到损害。

在软件行业，有三类人。你有赛车约翰尼，只关心他们的电脑工作，没有别的。

![](img/7e4e892e61c4febb5c9a73d7fbfa5834.png)

Photo from [https://knowyourmeme.com/](https://knowyourmeme.com/)

然后是不开发技术的技术爱好者。但了解这项技术是如何运作的。此人充当开发商和赛车约翰尼之间的中间人。

![](img/9fb7d48c419faf5160f323c755b5a687.png)

Photo from youtube.com

最后，你有开发人员/工程师，他们对应该如何使用技术有 sperg 级别的理解。他们可以创造新技术。

![](img/f1c848cd6d763c8d1108081f3130131a.png)

Photo from linux.com

这些安全风险的出现是因为维护软件安全的角色从技术爱好者/开发者直接转移到了赛车 Johnny 身上。赛车约翰尼不知道如何设置自动化代码审查。他也不会手动检查代码来检查恶意软件。他也没有设置自动安全更新。除此之外，[就像 Windows 一样，大多数恶意软件都被设计用来攻击最流行的服务和产品](/@dretechtips/why-windows-is-better-than-linux-da410b8d9689)。因此赛车手约翰尼将自己置于安全风险之中。这就是为什么销售给赛车约翰尼的软件[如 Windows 必须方便易用的原因。然而，便利是以自由和效率为代价的。因为该软件是为大众量身定制的。这就把我们带到了下一点。](/geekculture/signal-will-be-compromised-eb18a91fd51f)

## WordPress 网站效率低下。

WordPress CMS 在协作等功能方面效率低下。市场上现有的允许协作的软件，如 Google Docs。视频流技术允许多方相互交流播客。然而这些 WordPress 网站默认只允许一个用户修改他们的内容。你大概可以安装满足这些特性的插件。然而，这又把我们带回了最初的安全风险和膨胀问题。WordPress 有这么多问题，人们会问我们能做些什么。

## 你能做些什么？

这些问题始于设计层面。一般来说，b [广告设计需要大量的补丁和黑客来确保产品正常工作。而好的设计需要很少的技巧和花招来确保产品正常工作。](https://www.youtube.com/watch?v=RNFesa01llk)

解决这个问题的最好方法是将托管内容的**和从**修改内容的和**分开。大多数博客满意度管理系统的管理界面都位于人们公开访问信息的同一个服务器上。你的卫生间和餐厅分开是有原因的。您可以通过将管理界面锁定到单独的 IP 地址来获得类似的结果，但是，这并不能解决问题，因为可以利用漏洞绕过此类限制。**

解决方案是使用基于 Git 的内容管理系统。基于 Git 的内容管理系统使用 Git 系统来提供位于其中的内容。大多数开发人员都熟悉 Git 的功能。它允许对代码进行版本控制，但是，它也可以用于纯文本。这些提交可以运行持续集成工具，这些工具可以动态扫描代码库以查找恶意软件。自动向内容添加信息。一旦提交在 CMS 上得到处理，它就被直接推送到生产服务器，作为静态 HTML 文件供公众使用，带有 CSS 和 [JavaScript](/geekculture/the-javagate-scandal-fead695c4830) 。

有多重好处。第一个主要好处是开源内容。你可以选择将开源内容添加到你的博客中，这在内容生产率方面是一个优势。大多数人成为数字农民的原因是他们只使用一个平台来执行一个功能。例如，你想写博客。但是什么是博客呢？它只是通过网站将信息从作者那里传递给读者的一种方式。有如此多的提供者提供实现。那么，为什么要依赖一个平台来执行这样的功能呢？你可以通过在多个平台上发布你的内容来解决这个问题。这[防止了去平台化的问题，因为一切都只是源代码的副本。](https://www.glamour.com/story/donald-trump-social-media-bans-twitter-facebook)

使用这样一个系统的唯一缺点是你将有一个更高的学习曲线。然而，一旦你通过了学习曲线上的某一点，你就会受益。但这也适用于其他技术领域。

正如您所看到的，关注点的分离减少了膨胀，保护了服务，并增加了有用的特性。

## WordPress 网站还有用例吗？

是的，WordPress 站点仍然有用例。WordPress 是最流行的内容管理系统。所以它拥有最多的插件。所以可能有好的主题和插件可以使用。一旦你制作了这个站点，它应该被用来将 WordPress 站点导出到静态网页模板中。静态网页很难黑。即使你试图黑掉静态网页，你也只能黑掉公共信息。有重要的动态功能，如电子邮件收集，需要存在。然而，这些可以外包给不同的第三方公司。

有些人可能会说，将动态元素直接外包给其他公司只会将安全风险转移到更大的目标上。你看，网络安全不是一个二元问题。只要时间足够长，任何公共系统都会被黑客攻击。给你的问题。您认为自己具备保护在线基础设施所需的网络安全知识吗？你愿意手动和自动检查安装在你系统中的代码吗？对大多数人来说，答案是否定的。所以对正常人的一般建议是外包他们的动态元素。对于了解网络安全的人来说，自托管可能更安全。因为更小的目标意味着更少的人试图妥协。

出于隐私考虑，建议你选择商业模式不围绕收集和出售用户数据的公司。在 T2，有些公司每月向使用他们服务的人收取一定的费用。作为回报，他们不会出于盈利目的出售客户数据。

## 最后

IT 和工程领域是快速发展的领域。跟不上意味着你将被落在后面。跟上的最好方法是保持最新的新闻和教育内容。[订阅免费电子邮件列表，将你的职业生涯提升 10 倍。](/subscribe/@dretechtips)

**加入我们，因为 50 多名想要快速提升职业生涯和知识基础的人已经注册。**

达到这一点可能意味着你同意大部分已经写了。留下大量掌声，让算法向大众推广更好的软件实践。

**相关内容:**

*   [停止为网飞和 Spotify 付费](/@dretechtips/how-to-build-your-own-illegal-streaming-service-ff353ef70cd0)
*   [你的密码没用了](/geekculture/your-passwords-are-useless-5087cdcb1433)
*   [如何建立一个基于内容的分发网络？](/@dretechtips/how-to-build-a-based-content-delivery-network-e1aa8bb237b3)