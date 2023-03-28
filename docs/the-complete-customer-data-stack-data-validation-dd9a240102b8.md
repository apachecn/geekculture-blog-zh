# 完整的客户数据堆栈:数据验证

> 原文：<https://medium.com/geekculture/the-complete-customer-data-stack-data-validation-dd9a240102b8?source=collection_archive---------31----------------------->

![](img/1cbd723c26839a07bf3678a543fdd09c.png)

# 处理杂乱的客户数据

如果你和客户数据打交道，你就知道它很乱。不仅有许多数据来源(手机、网络等。)，但是随着产品的成长和发展，客户数据也会发生变化。虽然进展对业务有益，但它也给负责向下游团队和系统交付干净数据的工程师带来了独特的挑战。

当脏数据顺流而下时，就会造成严重破坏。客户收到了错误的电子邮件，营销报告被破坏。2017 年， [Gartner 的研究](https://www.gartner.com/smarterwithgartner/how-to-create-a-business-case-for-data-quality-improvement/)确定，脏数据造成了 1500 万美元的年度损失，而且这一数字自那以来肯定没有下降过。

数据工程师的角色至关重要，但数据验证是一个难题，尤其是在日益复杂的数据堆栈环境中。

在本帖中，我们将确定数据验证的常见挑战，并向您展示 RudderStack 如何使它成为您工作流程中的一个平稳步骤。

# 分解挑战

# 对事件数据中预定义模式的更改

数据工程师必须确保从构成单个事件模型的所有不同组件、平台和软件发送的事件数据符合预定义的模式。这确保了下游的一切都按预期工作。

通常，使用数据的分析和数据科学团队需要定义这个模式。通常，他们会使用电子表格来维护它。他们将与数据工程师和为事件流编写代码的开发人员共享这些电子表格。

这种工作流程适用于简单的小规模项目，但快速发展的公司或大公司必须处理更复杂的问题。不可避免地，决策会产生不符合模式的数据。考虑这些场景:

*   **该企业收购了另一家公司。**新公司的客户数据模式有所不同，但销售、营销和支持部门仍然需要尽快将这些数据纳入他们的工具中。通常根本没有足够的时间来重做整个验证工作流程。
*   **营销实现新工具，不涉及数据工程**。突然，管道正在发送“无效”数据，而这些数据实际上对下游团队至关重要。

# 为了兼容性而牺牲完整性

如果所有的事件数据都存在于每一个下游工具中，对于数据工程师来说，这将会简化很多事情。但是在现实世界中，每个目的地都是不同的，许多下游工具不会接受有效载荷的每个部分。

这意味着要进行权衡。为了成功地将它们发送到特定的下游目的地，通常你必须从有效载荷中丢弃或修改属性或特征。

这里一个主要的考虑是保持原始的有效载荷。在许多情况下，您会希望保留一份原始有效负载的副本，以作为“真相来源”参考，而不是查看由下游工具解释的真相版本。

# 确定脏数据不符合的原因并解决根本问题

也许数据验证最具挑战性的部分是理解数据变脏的原因和方式，然后从根本上解决问题。

您需要一个捕获脏数据的机制，一个无模式的存储/死信队列，它允许您访问和分析脏数据。理想情况下，你还会利用一个警报系统来帮助你避免堆积。因为当数据出现问题，事件开始堆积，下游后果变得更糟，清理时间增加。

# RudderStack 的 API 使得数据验证更加容易

我们与客户密切合作，构建了两个使数据验证更容易的 API。

首先是我们的**数据治理 API** 。它能够详细观察通过管道的每个模式。很快，我们还将发布允许您定义模式、验证事件以及阻止或丢弃不符合要求的有效负载的特性。

其次是我们的**转换 API** 。它使得为下游系统修改有效负载就像编写 JavaScript 函数一样简单。

这两个 API 都被设计成适合您现有的 CI/CD 工作流并利用 git。让我们深入细节。

# 数据治理 API

RudderStack 为您提供了不同的模式版本，您可以根据系统中的每个事件模型进行观察。我们计算通过的每个事件的事件模式，并以结构化的方式编译它们，您可以通过我们的[数据治理 API](https://docs.rudderstack.com/rudderstack-api-spec/rudderstack-data-governance-api) 访问它们。

每当给定事件模型出现上述任何问题时，都会生成一个新的模式版本以及各种见解，例如:

*   每个模式的总计数
*   用该模式捕获的几个示例事件
*   常见值及其频率

这允许您快速识别问题，并帮助您做出元数据驱动的决策。

## 例如

只要 event_model 的名称发生变化，就会生成一个新的事件模型，并通过[事件模型 API](https://docs.rudderstack.com/rudderstack-api-spec/rudderstack-data-governance-api#schemas-event-models) 提供。

使用相同的 event_model 名称，如果您最终对来自两个不同检测源的列使用不同的大小写，则会生成一个新的模式版本，并可通过 [Event Versions API](https://docs.rudderstack.com/rudderstack-api-spec/rudderstack-data-governance-api#schemas-event-versions) 获得。

**Pro Tip:** 设置一个警报或 cron，通过轮询数据治理 API 来报告/跟踪 RudderStack 观察到的任何新的事件模型或模式版本，这样您就可以掌握观察到的数据变化。

想要在一个地方获得所有数据平面的模式版本，以了解当前有多少个版本是活动的？查看来自[](https://github.com/davecook88)**的[舵栈模式比较表](https://github.com/isvictorious/rudderstack_master) _ *。***

*有关数据治理 API 如何简化数据验证的更多信息，请阅读我们的[这里的](https://rudderstack.com/blog/rudderstacks-data-governance-api)深度探讨。*

# *转换 API*

*RudderStack 的转换特性允许您利用定制的 JavaScript 函数将事件转换成特定于目的地的格式。使用转换 API，您可以通过 HTTP API 调用对您的转换执行各种操作。使用它来:*

*   *版本控制您转换，*
*   *在发布之前将它们存储在组织范围内的沙箱中，*
*   *利用转换库——您可以在转换中使用的模块化、可重用的 JavaScript 块，以及*
*   *检查新转换的编译和执行是否成功。*

*关于转换 API 的更多细节，请阅读我们的[这里的](https://rudderstack.com/blog/rudderstacks-transformations-api)深度探讨。*

# *免费注册并开始发送数据*

*测试我们的事件流、ELT 和反向 ETL 管道。使用我们的 HTTP 源在不到 5 分钟的时间内发送数据，或者在您的网站或应用程序中安装我们 12 个 SDK 中的一个。[入门](https://app.rudderlabs.com/signup?type=freetrial)。*

*本博客最初发布于:
[https://rudder stack . com/blog/the-complete-customer-data-stack-data-validation](https://rudderstack.com/blog/the-complete-customer-data-stack-data-validation)*