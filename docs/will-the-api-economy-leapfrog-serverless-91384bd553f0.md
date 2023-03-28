# API 经济会跨越无服务器吗？

> 原文：<https://medium.com/geekculture/will-the-api-economy-leapfrog-serverless-91384bd553f0?source=collection_archive---------15----------------------->

平台服务如何改变目标

![](img/30e551620b71c5451e58c4587dcac442.png)

Photo by [丁亦然](https://unsplash.com/@yiranding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/collections/3133689/learning-economy-?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

无服务器的采用主要是由希望在创纪录的时间内为最终用户提供价值的团队推动的。无服务器试验场沉浸在消耗战中，管理开销将被摧毁。这场革命中一个经常被忽视的方面是不断扩大的云服务矩阵，必须组合这些服务才能创建最佳实践的无服务器架构。由此产生的基础设施即代码(IaC)交付的价值很小，并带有巨大的风险。漂移、未编入预算的支出、有缺陷的设计和未能阅读服务限制的细则是 IaC 如何成为无服务器成功的对策的几个例子。驱逐 IaC 同时仍然利用云服务跳板的团队将发现自己拥有显著的竞争优势。进入 API 经济。

API 经济由需要零 IaC 的平台服务组成。这些服务通常在客户端基础设施之外运行，只需要一张信用卡就可以开始使用。以下是一些按使用情形分类的示例:

*   持久存储:[动物群](https://fauna.com/)、 [DGrapgh](https://dgraph.io/) 、[新贵](https://upstash.com/)
*   数据流:[事件](https://aiven.io/kafka)
*   Compute: [Fly](https://fly.io/) ， [GitHub](https://pages.github.com/) ， [Netlify](https://www.netlify.com/) ， [Gatsby Cloud](https://www.gatsbyjs.com/docs/how-to/previews-deploys-hosting/deploying-to-gatsby-cloud/) ， [Vendia](https://www.vendia.net/) ， [Vercel](https://vercel.com/)
*   分析: [Logtail](https://logtail.com/)

API 经济还可以包括 FourSquare、Google Maps 和 SalesForce 等产品的 API 集成层。

最近，像 Tim Wagner 这样有进取心的无服务器远见者已经创建了整体平台，为应用程序开发人员提供了一个最佳实践生态系统来启动他们的创业公司。例如，在 2020 年推出 Vendia 后，Tim 颠覆了现状，跳过 IaC，直接转向一种被称为“通用应用程序”或简称“Uni”的最佳实践应用程序。在注册 Vendia 的几分钟内，开发人员就会收到一个 GraphQL 端点、静态网站托管(S3)、消息传递(SQS 和 SNS)和一个无服务器的账本。不需要 IaC。

Vercel 通过提供一个完整的全球基础设施来运行 30 多个应用程序框架，为开发人员实现了从零到一的转变。这个健壮的基础设施允许应用程序直接利用服务器端渲染，而不需要一行 IaC。他们也有 API 经济服务的菜单，如直接与平台集成的[动物群](https://vercel.com/integrations/fauna)和[购物化](https://vercel.com/integrations/shopify)。

由于能够在输入信用卡的时间内从零到已部署的应用程序，开发人员社区已经开始想知道这将把无服务器留在何处。显而易见的答案是它支持 API 经济。无服务器 IaC 还支持 API 经济尚未进入的边缘情况。无法忍受 API 经济服务的不透明本质的企业也可以选择使用像[无服务器框架](https://www.serverless.com/framework)和[无服务器云](https://www.serverless.com/cloud)这样的工具直接进入云。无服务器云还将通过提供在创纪录的时间内部署的参考架构(无服务器组件)来改善开发人员的体验。但动力显然来自 API 经济。

度过启动阶段并进入扩展阶段的组织可能会选择 Kubernetes，而不是无服务器堆栈。无服务器仍然受到不可预测的定价、服务限制、冷启动时间、特定于供应商的控制平面以及无状态环境的一类问题的困扰，这些问题不会很快消失。随着组织的成熟，雇佣能够实现平台服务(通常由开源技术支持)的顶尖人才变得更加容易，这些平台服务是根据企业的独特需求定制的。这只需要租赁计算(即 EC2)和一些精英工程技术。许多组织经常创建开源软件来支持平台服务，如网飞、谷歌和脸书。这导致了内部 API 经济的产生，这种经济经常会回到公共领域。平台独立性和从特定于供应商的控制平面上迁移是成功的技术创业发展的一部分。保持合理设计原则的初创公司会发现，移除第三方 API 服务并用自己的服务替换它们，要比大规模重构容易得多。

像 [Dapr](https://dapr.io/) 这样的新兴技术使得组织比以往任何时候都更容易构建直接插入到他们的 Kubernetes 基础设施中的内部 API 经济。这种架构的好处是巨大的。团队得到一个由中小企业构建的即插即用架构，企业得到一个 Kubernetes 社区可以维护的低成本可移植基础设施。然而，这些技术应该在组织的扩展阶段实现。

只有时间能告诉我们 API 经济的真正命运。如果初创公司的成功率显示出适度的提高，预计未来五年的 API 经济增长将超过过去五年的无服务器增长。蒂姆·瓦格纳(Tim Wagner)等远见者在平台服务上下了大赌注，这是一个非常强有力的领先指标，表明 API 经济的时代已经到来。