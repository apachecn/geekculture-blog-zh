# 360 IT 检查#25 — Log4j、优化 React 等等！

> 原文：<https://medium.com/geekculture/360-it-check-25-log4j-optimizing-react-and-more-bb1d208a06ed?source=collection_archive---------22----------------------->

![](img/d23712a3d9ef463ce4bc419d53b23546.png)

# Log4j 漏洞

当您的日志框架开始执行任意代码时，您就知道自己有麻烦了。

流行的 Java 框架 Log4j 包含一个漏洞，[危及你可能正在使用的许多应用，包括 Twitter、iCloud 或 Steam。](https://techcrunch.com/2021/12/10/apple-icloud-twitter-and-minecraft-vulnerable-to-ubiquitous-zero-day-exploit/)这份名单比预期的要短得多。Log4j 可能出现在“几乎所有”主要的基于 Java 的企业应用程序和服务器中。甚至连国家安全局也不安全，因为广受欢迎的 GHIDRA 也因此变得脆弱。

据分析“互联网背景噪音”的公司 GreyNoise 称，大约有 100 台主机正在积极扫描存在上述漏洞的服务器。

为了检查你的服务器是否被扫描，你可以通过检查[GitHub gist](https://gist.github.com/gnremy/c546c7911d5f876f263309d7161a7217)来验证你的访客。然而，这些 IP 是 Tor 出口节点的 IP；因此它会变得更长。

幸运的是，修补漏洞的方法很简单。正如 Cloudflare 解释的那样:

> *1。升级到 Log4j v2.15.0*
> 
> *2。如果您使用的是 Log4j v2.10 或更高版本，并且无法升级，则设置属性:*
> 
> *log4j 2 . formatmsgnolookups = true*
> 
> *此外，可以为这些相同的受影响版本设置环境变量:*
> 
> *LOG4J _ FORMAT _ MSG _ NO _ LOOKUPS = true*
> 
> *3。或者从类路径中删除 JndiLookup 类。例如，您可以运行类似*的命令
> 
> *zip -q -d log4j-core-*。jar org/Apache/logging/log4j/core/lookup/jndilookup . class
> 从 log4j-core 中删除该类。*

# React 的自动优化

React 有点像“什么都要自己做”的 UI 库。如果没有显式的优化，比如内存化，它的性能会很差，不必要地反复运行大量代码。必须记住这一点，以及其他一些特定于 React 的优化，可能有点麻烦。

Meta 的工程师们似乎已经注意到了这一点，并展示了一个工具，让 React 的性能**更好**而没有开发人员的所有精神负担。解决方案是一个自动编译器，它会自动为您执行记忆。如需演示，请观看下面的视频。

# 线路泄漏

如果您曾经用您的私钥向一个公共的 GitHub 存储库提交，信不信由您，这并不是可能发生的最糟糕的事情。 [Line 向 GitHub 泄露了**13.3 万用户**的支付数据。](https://www.theregister.com/2021/12/07/line_pay_leaks_around_133000/)泄露的数据包含了“LINE Pay”推广计划参与者的详细信息:日期、时间和用户 id。尽管没有信用卡或银行账户的详细信息被公之于众，但它们本可以被[“不费吹灰之力就能追踪到”](https://www.theregister.com/2021/12/07/line_pay_leaks_around_133000/)

泄漏发生在 2021 年 9 月至 11 月之间；然而，关于它的消息上周才浮出水面。公司承诺未来会做得更好。

# 顺风 CSS 3.0

Tailwind CSS 是 web 开发人员最喜欢的创建漂亮 UI 组件的工具之一，它即将发布一个新的主要版本。有一些改进使开发人员的生活更容易，一些新的特性带来了新的功能。

*   “刚好及时，一直都是。”加快 CSS 框架的构建速度。JIT 引擎直到现在都是可选的，那时它是唯一可用的选项。它也可以作为一个脚本从 CDN 获得，并在浏览器中运行
*   [滚动快照 API 的介绍](https://tailwindcss.com/blog/tailwindcss-v3#scroll-snap-api)
*   无需花费太多时间或精力的表单元素的样式
*   [RTL 和 LTR 修改器](https://tailwindcss.com/blog/tailwindcss-v3#rtl-and-ltr-modifiers)的介绍。构建多方向网站时，对完全控制至关重要。
*   “触摸操作”实用程序，用于控制用户如何在带有触摸屏的设备上放大和滚动

关于新版本的完整特性列表，请看一下[文档](https://tailwindcss.com/docs/)。

360 IT Check 是一份周刊，在这里我们为您带来世界上最新最棒的技术。我们涵盖了新兴技术框架、创新创业公司的新闻以及其他直接或间接影响科技世界的话题。

喜欢你正在读的东西吗？请务必订阅我们的[每周简讯](https://www.itmagination.com/newsletters/360-it-check)！

*原载于*[](https://www.itmagination.com/blog/360deg-it-check-25-log4j-react-line-tailwind-css)**。**