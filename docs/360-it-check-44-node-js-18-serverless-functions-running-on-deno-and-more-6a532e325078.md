# 360 IT Check #44 — Node.js 18、Deno 上运行的无服务器功能等等！

> 原文：<https://medium.com/geekculture/360-it-check-44-node-js-18-serverless-functions-running-on-deno-and-more-6a532e325078?source=collection_archive---------13----------------------->

![](img/656f25ade5e80c0ab4830a925bfbf39b.png)

# 节点 18

上周，我们看到了 JavaScript 运行时的新更新。18 版本并没有引入太多新功能。事实上，我们只见过其中的两个。不管有没有，Node.js 社区已经预料到了。我们也看到了一些突破性的变化，包括移除对 32 位 Windows 机器(！idspnonenote)的支持。).虽然我们很少看到运行该版本操作系统的机器，但仍有一些运行该版本的机器。尽管外部因素占了上风，我们还是目睹了大量计算机失去支持。

更多详情，请参见我们对 Node.js 18 的详细概述。

# 年轻的 JavaScript 兄弟赢得了胜利

Deno 是 Node.js 的弟弟妹妹(他们都有同一个父亲 Ryan Dahl)。[在创建 Node.js](https://www.itmagination.com/blog/the-complete-history-of-javascript-typescript-and-node-js) 之后，达尔有很多遗憾。更糟糕的是，如果不彻底重写，他最初做出的一些决定就无法改变。他在第二个著名的 JavaScript 和 TypeScript 工具中解决了这些问题:

虽然专门用于 Deno 的软件包的生态系统要温和得多，[有一种方法可以在它的兄弟上运行 Node.js 代码。](https://deno.land/std/node/README.md)

上周， [Netlify 宣布了一件大事](https://www.netlify.com/blog/announcing-serverless-compute-with-edge-functions):他们的无服务器计算功能可以在两个 [JavaScript](https://www.itmagination.com/full-stack-javascript-development-in-2021) 工具中较新的一个上运行。该公司甚至明确强调了他们服务的动力。

**底线**

明确地将 Deno 展示为发电站是有趣的:毕竟，它远没有 [Node.js](https://www.itmagination.com/blog/node-js-changed-corporate-software-engineering) 受欢迎。这肯定是一个故意的行动；或许目的是让产品与众不同。不可否认，无服务器功能的市场已经相当拥挤，AWS、Azure 和 GCP 主导了这个领域。然而，上述三个平台并没有提供 Netlify 所提供的功能。做小池塘里的大鱼的策略是一个被证明有效的策略。

从另一个角度来看，这是 Deno 社区的胜利。对替代工具的支持有点平淡无奇，也许我们可以从最初发布时的水平上看到热情。

# Kotlin 和 Ktor，每天处理 700 亿个事件

Ktor 是在 [Kotlin](https://www.itmagination.com/blog/360deg-it-check-7-kotlin-10-years-cybersecurity-talent-stack-overflow-survey-pixel-6-tensor-ai-chip-windows-365) 中构建后端的框架。事实证明，这种组合是相当可扩展的。事实上，它的扩展性非常好，据 Adobe 称，它每天可以处理 700 亿个事件。也就是说，每小时近 30 亿个事件，每分钟近 4900 万个事件，每秒钟略高于 80 万个事件(EPS)。这是一个相当令人印象深刻的分数。

如果你想了解更多，请看下面的视频:

**底线**

构建一个服务并让它运行良好是一回事。然而，在某一点上，当涉及到多少连接、事件等时，你的应用会碰壁。，它将能够处理。当然，任何人都可以在服务器上投更多的钱。关键是能够编写一个应用程序，能够在给定的时间内处理最多的用户，而不需要花费太多。正如这个案例所显示的，Ktor 是伸缩性很好的框架之一，例如， [Node.js](https://www.itmagination.com/blog/360deg-it-check-44-nodejs-18-netlify-deno-kotlin-ktor-ubuntu-canonical) 也是如此。

# 新的 Ubuntu 22.04 LTS 版本

Ubuntu 是许多人选择的入门级系统，然而，更重要的是，它通常是应用程序部署的首选操作系统。上周，这个流行的 Linux 发行版背后的公司 Canonical 发布了 Ubuntu 的新版本，以及一个长期支持版本。

**底线**

公司经常会在 LTS 版本之间迁移。这一次，这将减少更新的频率，同时保持应用程序的安全性。虽然这种方法不允许使用最新的功能，但它将提供一种稳定感，并允许团队专注于日常工作，而不是担心过渡到另一个版本的操作系统。

*最初发表于*[*https://www.itmagination.com*](https://www.itmagination.com/blog/360deg-it-check-44-nodejs-18-netlify-deno-kotlin-ktor-ubuntu-canonical)*。*