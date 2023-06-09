# 扩展测试者权利法案

> 原文：<https://medium.com/geekculture/expanding-on-the-tester-bill-of-rights-cae6d43a6491?source=collection_archive---------14----------------------->

## 详述作为敏捷项目团队的测试人员意味着什么。

![](img/e045fde83e0fcb1aa7520118616b2912.png)

Photo by [David Travis](https://unsplash.com/@dtravisphd?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我一直在通读 Lisa Crispin 和 Janet Gregory 的 [*《敏捷测试:测试人员和敏捷团队实用指南*](https://www.amazon.com/Agile-Testing-Practical-Guide-Testers/dp/0321534468) ，当我发现有一小部分讨论了程序员和客户的“权利法案”时。Lisa Crispin 注意到缺少“测试人员权利法案”,所以她和 Janet 着手创建一个。

“测试人员权利法案”包含六条，每一条都描述了敏捷团队中测试人员的主要权利或责任。不幸的是，每一条都缺乏细节。新测试人员可能会发现自己对这些文章有疑问，特别是:

1.  为一个 sprint 故事提供估算意味着什么？
2.  为什么我应该提倡“整个团队”的质量方法？
3.  我如何向我的团队介绍采用新工具？

本文试图回答上述问题，同时对珍妮特·格雷戈里和丽莎·克里斯平最初撰写的文章提供见解。

# 第一条

> "您有权在任何时候提出与测试、质量和流程相关的问题."

作为一名测试人员，在整个 sprint 过程中的任何时候提出与应用程序质量相关的问题是您的首要任务。这些可以是 bug、设计查询、敏捷实践，或者任何其他可以成为持续改进目标的项目。

当我在 TransLoc 担任 QA 自动化工程师时，我注意到我的团队的整理过程更侧重于充实故事，而不是精炼积压。这导致团队花更多的时间讨论故事，最终以每次会议“整理”一个故事结束，而我们应该做至少五个。结果，我们每周举行两次培训会议，占用了开发和测试时间。

我向我的团队提出了这个问题，并和我的 scrum 主管讨论了潜在的解决方案。我们决定，我们将严格按照细化时间来测试培训，同时要求故事作者至少提前一周充实他们的门票。结果是效率的净增长，因为我们能够将花在梳理和讨论门票**上的时间减少一半**。

尽早地、经常地说出你看到的问题，即使它们与质量和测试没有直接关系。

# 第二条

> “您有权向客户、程序员和其他团队成员提问，并获得及时的回答。”

我个人对屈服于冒名顶替综合症和在验票时不知道的事情感到内疚。尽管我在这个行业已经呆了五年，但我还没有戒掉这个丑陋的习惯。这导致我在开发票上浪费时间，而开发票本来是很简单的。解决办法？

不仅向你的开发团队提问，也向产品干系人和客户提问。

我最喜欢的质量保证要素之一是与我的开发团队合作。我是“案头检查”的支持者，在办公室环境中，我会给正在开发某个功能的开发人员发消息，询问我是否可以顺便去聊聊进展如何。开发人员向我介绍他们到目前为止已经实现了什么，我会花时间询问任何与应用程序质量相关的问题。

应该达成一致的是，在案头检查时没有愚蠢的问题。相反，您应该可以随意提出任何问题，无论问题的大小，都可以帮助您更好地理解该特性。

我还会花时间与产品所有者和客户交谈，以更好地确定我们的用户如何使用该应用程序。这不仅有助于确定关键的工作流程或主要的自动化候选方案，也有助于成为客户更有力的支持者。

你应该感到有权力向你的团队或客户询问任何问题，这将有助于你更好地理解应用程序及其使用方式。

# 第三条

> 你有权向项目团队中的任何人寻求帮助，包括程序员、经理和客户

我最近遇到了一个情况，我被拉得很瘦，无法独自对我所有的发行票进行可行的回归测试。我有两个选择:

1.  单独测试所有的东西会有推迟发布的风险
2.  请我的团队和我一起测试

我选择让我的团队帮我测试，我很清楚他们和我一样忙。我们的发布如期进行，我们能够为客户提供价值。

我知道对于许多测试人员来说，接近一个开发团队并寻求帮助进行测试，或者理解一个特性是如何实现的是很困难的。我亲身经历过这种情况，尤其是在开始新团队或新公司的时候。推动我前进的是这样一种认识:如果我不寻求帮助，我可能会成为我团队的绊脚石。

尽你所能帮助你的团队。需要时寻求帮助，无论是测试票证、阐明功能实现还是理解流程。

# 第四条

> "你有权评估测试任务，并把它们包含在故事评估中."

我无法告诉你我有多经常听到测试人员说他们在自动化或者测试中的努力没有被纳入到故事评估中。简而言之，这是一个反模式，如果失去控制，可能会降低团队的冲刺速度。

虽然很难估计测试项目的时间、t 恤尺寸或工作量(无论您的团队遵循哪种实践),但是您应该尽最大努力为团队提供一个大概的数字。测试人员在评估时最关心的是如何量化他们花费在自动化和测试上的时间。

例如，您有一个估计为 5 个开发故事点的票证，这应该是大约 2-3 天的工作。根据产品的历史记录，对票证的测试大约需要半天时间。自动化是与开发同时进行的，并且可能需要一天的时间来完成。

根据我的经验，我会把这个故事评为 8 级。我个人会为测试增加 0.5 分，为自动化增加 1-1.5 分，结果是 7 四舍五入为序列中的下一个斐波纳契数 8。

评估您的测试活动。它不仅会让您的开发团队更好地了解测试需要多少努力和时间，还会为您的速度测量带来更多的准确性。

# 第五条

> "您有权获得及时执行测试任务所需的工具."

作为一名测试人员，您应该感到有权利要求可以帮助您完成工作的工具，无论是开源工具还是订阅工具。最好的方法是向你的领导团队展示数据，说明为什么这个工具对你的日常工作是必要的。

当我开始在 Vodori，Inc .工作时，开发团队使用 Hiptest(现在的 Cucumber Studio)的免费版本，当时一次只允许 10 个用户。当开发人员参与编写和执行测试场景时，我们的团队不断地添加和删除用户。这是一个相当麻烦和耗时的过程。

在与免费版角力了大约六个月后，我提出 Vodori 应该考虑企业版。为了陈述我的情况，我联系了 Hiptest，要求进行一次试验。他们给了我们一个月的企业版，条件是一个月后，我们要么需要付费，要么降级到免费版。

我画出了我们花在删除和添加许可证上的时间与企业版成本的对比图。我还注意到了使用 enterprise 的好处，比如访问“动态文档”。然后，我将这些数据提交给我的 QA 经理和公司的 CTO，认为使用 enterprise 可以节省时间和精力。

我的案子被拒绝了。简单地说，Vodori 当时没有能力依赖企业软件。我们只是没有足够的资金来证明这笔开支的合理性。

你不应该害怕寻求可以帮助你更好地完成工作的工具。陈述你的情况，向领导展示数据，如果他们拒绝你的请求，表现得优雅一些。

# 第六条

> “你有权期望你的整个团队，而不仅仅是你自己，对质量和测试负责。”

这很可能是“测试者权利法案”中最重要的条款。你的团队应该理解质量是整个团队都应该努力的，而不仅仅是那些负责产品测试的人。

我在 Pendo.io 的团队一直在努力将质量从右侧泳道带入左侧泳道。我们不像瀑布团队那样在冲刺的最后关注质量，而是尽可能早地讨论质量问题。我们目前已采取以下措施来确保质量是“整个团队”的努力:

1.  定期讨论 Slack 中的应用程序质量和设计
2.  邀请测试人员参加蓝图、培训和计划会议
3.  在 epic 蓝图、整理和规划期间记录质量影响
4.  参与手工和自动化测试的开发人员
5.  关于如何测试以及测试时要寻找什么的团队论坛

正如 Lisa Crispin 和 Janet Gregory 在*敏捷测试*中多次强调的，质量是“整个团队”的行动，而不仅仅是测试方的责任。

# 摘要

你不可能知道所有的事情，尤其是关于新特性的实现。在这种情况下，向你的同伴提问来巩固你的知识。记住你不是在一个孤岛上，当你觉得有必要的时候，随时向你的同伴寻求帮助。在 sprint 故事中评估高质量的任务，这样它们可以更准确地描述你的团队的速度。这将有助于调整你的短跑。

要求合适的工具，但要准备好向领导展示数据，说明你为什么需要它，它能为你做什么。询问应用程序质量、过程和测试，以便持续改进。最重要的是，记住质量是团队的努力。

# 资源

1.  *敏捷测试:测试人员和敏捷团队实用指南*，Lisa Crispin 和 Janet Gregory，Addison-Wesley，2014 年，第 50 页。
2.  “活文档”*生活文档| CucumberStudio 文档*，[support . smart bear . com/cucumber studio/docs/BDD/Living-doc . html](http://support.smartbear.com/cucumberstudio/docs/bdd/living-doc.html)。
3.  *敏捷测试:测试人员和敏捷团队实用指南，*第 15 页。