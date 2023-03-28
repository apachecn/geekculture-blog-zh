# FeatureHub 简介—开源功能标志服务

> 原文：<https://medium.com/geekculture/introducing-featurehub-open-source-feature-flag-service-93e58596f778?source=collection_archive---------29----------------------->

![](img/a9256fe03ad45d3fcd972a813b11a280.png)

# FeatureHub 背后的一点历史

在过去的几年里，我和许多客户一起工作，他们一直在努力决定选择哪种特性标记平台。通常归结为成本和不愿意承担额外费用。对于那些做出总体决策的人来说，似乎很难量化产品中的特性标志和实验的价值，这可能是因为这种技术仍然没有得到广泛使用，并且没有将其作为标准实践嵌入所需的认同。因此，我合作过的许多团队都构建了他们自己的特性标记服务，这样的决定来自开发人员自己。当你可以构建一个服务并完全控制它时，这是一件很棒的事情，然而很多团队没有意识到的是功能标志是一项非常复杂的技术，它需要随着时间的推移而发展，并且像所有产品一样需要不断的增强和维护。因此，软件团队最终会构建一个非常简单的解决方案，而不一定适合整个团队。例如，它可能是一个没有接口的功能服务，因此您的“业务人员”永远无法在生产中打开或关闭某个功能，这实际上违背了整个方法的目的。另一个问题是，it 通常没有重要的功能，如逐步部署和分析，以执行更复杂的 A/B 场景，更不用说企业级安全支持了。除此之外，如果您的企业使用多种技术和编程语言，他们很可能会构建多种功能标记服务，而不是为整个公司提供一种通用服务。

大约一年前，我和其他工程师一起研究这些问题，我们想到了构建一个适用于任何团队的特性管理工具的想法。我们还想让它成为开源和免费的自托管解决方案，易于安装、维护并与 CI/CD 管道集成。这可能是一个雄心勃勃的目标，但我们的目标是抓住那些接受开源、DevOps 和云原生技术的团队，并使其成为一个适合小型初创企业、中型企业和大型企业组织的“首选”功能管理和实验平台。

一年后— *FeatureHub* 诞生。

# 什么是 FeatureHub？

[**FeatureHub**](https://www.featurehub.io/) 是一个云原生和开源的特性管理和实验平台。它附带了 FeatureHub 管理控制台—控制您的功能的 web 界面，以及各种帮助您将 FeatureHub 与您的代码连接起来的 SDK。它是为开发人员、测试人员、业务人员或团队中任何想要控制产品特性、发布管理和测试的人而设计的。

您支持什么类型的功能？

目前，FeatureHub 平台支持特性标志、字符串、数字或更高级的 JSON 格式，用于各种形式的配置。

**是否支持远程配置，比如 Firebase？**

是的，我们支持 JSON 远程配置，可以像 Firebase 远程配置一样使用。我们还提供移动 SDK。

**我能把它用于 web、API 和移动应用吗？**

我们的目标是构建一个通用平台来支持所有类型的应用，因此我们有各种 SDK 来支持这一目标。

> 目前，我们支持以下 SDK 和应用类型:
> 
> Java，Java-Android，Javascript/Typescript，Node js，React，C#，Go，Python，Ruby，Dart，Flutter Mobile，Flutter Web

**我可以向一定比例的用户群或指定的用户 id(电子邮件地址)部署功能吗？)**

是啊！从版本 1.1 开始，您可以根据您的独特要求，基于百分比或目标规则(如用户密钥、国家/地区、平台、设备、版本或自定义规则)逐步推广(拆分)。

**安全吗？我能在银行使用它吗？**

FeatureHub 旨在确保安全性从一开始就构建到平台中。如果我们把它分为认证和授权特性，Hub 提供了两种主要的认证方法:登录，通过它某人能够获得一个不记名令牌，然后使用 API，或者服务帐户，通过它 SDK 能够读取(并且可能)更新特性的状态。身份验证由内置机制或 OAuth2 提供(目前我们支持 Google、Microsoft 和 GitHub 的登录)。默认情况下，不记名令牌存储在数据库中，并在每次请求时进行验证——预计我们将在需要时为 Memcache 和 Redis 实现这一点。

在路线图上，我们计划支持其他 OAuth2 供应商，以及那些偏好 SAML 的企业环境。

**我能看到一些关于我的功能表现的分析吗？**

我们一直认为 FeatureHub 不仅仅是一个功能标记平台，而是一个尽可能早、尽可能频繁地向你的团队提供反馈的工具。在生产中进行测试并允许反馈循环是我们在构建 FeatureHub 平台时遵循的基本概念之一。为了实现这种可见性，我们提供了现成的谷歌分析支持。使用此功能，您可以设置事件，并根据功能状态跟踪这些事件的执行情况。点击阅读更多相关信息[。](https://docs.featurehub.io/analytics.html)

**我以前都听说过，你有其他一些很酷的功能吗？**

*   FeatureHub 包含了额外的功能，为开发运维团队提供了更好的可视性。例如，**“环境升级订单”**允许您根据您的测试和发布流程订购环境。您将能够以与测试和发布功能完全相同的顺序，在一个仪表板中查看所有环境中功能标志的状态。
*   **锁定/解锁**功能是我们为了增加安全性而内置的，这可以防止人们在功能尚未准备好开启的情况下意外开启功能。例如，如果您是开发人员，您可以在生产中锁定该功能，这样产品所有者就会知道他们还不能启用该功能。您还可以使用它来为其他环境中的测试人员指示准备就绪。

**怎么会是免费的？**

FeatureHub 是一个自托管平台，通常您会将它安装在自己的基础设施上，因此我们不承担任何托管成本。因为我们使用 Docker 容器，所以您可以在自己的基础设施上轻松安装和运行它。

**易于维护/升级/支持吗？**

FeatureHub 打包成 Docker 容器。每个服务都是一个微服务，管理自己数据库的模式更新。当您升级 FeatureHub 容器时，它将锁定模式表并升级数据库。如果这需要停机，我们会在不同的版本中指明。发布说明可以在[这里](https://github.com/featurehub-io/featurehub/milestones?state=closed)找到。

**路线图上有什么？**

其他语言 SDK、**功能审计**支持以及与其他第三方服务的各种集成。

FeatureHub 平台易于安装、配置和与您的应用一起使用。我们确保 FeatureHub 能够满足开发人员、测试人员、商业用户以及任何希望了解工程团队开发的产品特性的人的需求。云原生方面也使得在任何云中或内部托管和扩展 FeatureHub 以及使用 Docker 和 Kubernetes 运行它变得容易。如果你正在为你的敏捷或开发团队寻找一个开源特性管理软件，试试 [FeatureHub](http://featurehub.io/) 吧。

***如果你热衷于尝试 FeatureHub，请阅读下一篇文章，解释*** [***技术细节***](https://irinasouthwell-220.medium.com/introducing-featurehub-open-source-feature-flag-management-and-experimentation-platform-f6dba7418dc5) ***。***

如果您想投稿或查看我们的主要 FeatureHub Git 资源库，请访问[这里](https://github.com/featurehub-io/featurehub)。

有关文档，请访问[功能中心文档](https://docs.featurehub.io/index.html)页面。

# 阅读更多关于 FeatureHub 的文章

[使用 FeatureHub 免费管理功能标志——实用指南](https://irinasouthwell-220.medium.com/introducing-featurehub-open-source-feature-flag-management-and-experimentation-platform-f6dba7418dc5)

[使用特征标志的测试自动化](/@irinasouthwell_220/test-automation-with-feature-flags-fd75b252b655)

[带有 React、FeatureHub 和 Catch &释放模式的客户端功能标志](/@irinasouthwell_220/client-side-feature-flags-with-react-featurehub-and-catch-release-mode-cb4722e9928d)

[在版本 1.0 中引入百分比分割](https://www.featurehub.io/post/featurehub-releases-version-1-0-0-to-support-gradual-percentage-rollout-and-a-b-testing)

[在版本 1.1 中引入目标规则支持](https://www.featurehub.io/post/introducing-targeting-rules-support-in-version-1-1)