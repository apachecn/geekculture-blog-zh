# 该不该换微服务？

> 原文：<https://medium.com/geekculture/should-you-change-to-microservices-53c8527e99b7?source=collection_archive---------10----------------------->

在这个新时代，巨石柱被认为是原始的，但它们是真的吗？

![](img/5fd23028227f2a5db7e1ac3d542e7ef0.png)

欢迎，感谢你点击这篇文章，我希望你会喜欢它，最重要的是，它会有所帮助。因此，在我们开始之前，让我向您介绍一下本文的主题:

*   什么是微服务？
*   支持/反对微服务的案例
*   微服务的利弊
*   微服务应该有多大？
*   如何向微服务转变？
*   结论

## 什么是微服务？

您可能听说过微服务这个术语，它们有多棒，您可能对它的含义有一个模糊的概念，但是微服务到底是什么呢？嗯，这是一种架构模式，在这种模式下，您可以将您的应用程序构建为较小组件(服务)的集合，这些组件松散耦合、可独立部署、围绕业务功能组织、高度可维护和可测试，并且由一个小团队拥有。本质上，使用微服务，您可以将未来的整体分割成独立的业务组件，如下图所示。

![](img/3bcb06578b07f1edbcf256004fa9d9da.png)

A simplified view of monolith vs microservices

借助微服务架构，您可以将一个大型应用拆分为多个应用来处理特定任务，而不是维护一个大型应用。在一个非常简单的例子中，您可以想象一个电子商务应用程序:您可以将您的应用程序分成 3 个应用程序，而不是一个整体:购物服务、订单服务和运输服务。

客户通过浏览器连接到您的购物服务并购买一件商品。购物服务然后将告诉订单服务关于订单的信息，后者然后将购买的所有细节存储到客户配置文件中，然后告诉运输服务有一个新订单需要运输。

现在，您可能会问自己:“好吧，我刚刚将我的应用程序分成了 3 个不同的应用程序，现在我必须处理一个分布式系统和更多的复杂性，因为我必须在应用程序之间执行非常可能的异步任务”。你是绝对正确的，这一点从一开始就要弄清楚，这一点很重要。并不是每个项目都应该遵循微服务的道路，有一些特殊的情况需要走这条路，但在大多数情况下，你会做得很好。**微服务不是发展的银弹。**

## 微服务案例

早在 2008 年，用一个庞然大物为全世界服务的网飞意识到他们需要一个更好的解决方案，因为每次他们部署一些更改时，他们都必须呆在他们所谓的“作战室”中检查是否有问题，如果有问题，他们必须找出是哪部分代码导致了问题。有一次，一个丢失的分号让他们的网站关闭了几个小时。当您更改为微服务时，您处理的是更小、更易管理的代码库，因此检测错误变得更容易，即使一个 bug 设法潜入产品，也只有该服务会出错。

## 针对微服务的案例

Segment，一个客户数据平台(CDP)将他们的整体架构分解为微服务，希望有一个更光明的未来，但几年后，他们最终又回到了整体架构。基本上发生的情况是，当微服务试图纠正问题时，任何一个路由连接中的错误都会导致请求堆积。随着越来越多的请求堆积起来，系统会尝试通过自动扩展进行补偿，从而导致端口过载，进而给客户群带来性能问题。这可能是由架构的错误实现引起的，但您可以看到微服务架构如何成为一个更大的问题，而不是有所帮助。

## 微服务的优势

在我们讨论微服务的负面影响之前，我想指出这种架构风格的巨大好处。

*   **微服务是可独立部署的:**每项服务都是独立部署的，它允许您持续改进并向您的客户交付该服务，而无需重新部署其他一切。
*   **微服务是独立可扩展的:**通常在一个 monolith 中，如果你想扩展，你基本上是千篇一律地将整个 monolith 粘贴到不同的机器上，这大大增加了工作量。借助微服务，您只需扩展最需要的资源，从而简化扩展过程。
*   **微服务通过故障隔离减少停机时间:**如果某项服务出现故障，您可以轻松隔离故障，防止故障蔓延到管道的其他部分。在您的关键应用程序出现任何问题或客户注意到问题之前，有效地为您提供了额外的时间来修复故障。
*   **较小的代码库允许团队快速理解代码:**微服务通常有较小的代码库，即使团队是服务的新手，它们也更容易阅读。他们可以快速赶上并维护和部署服务。此外，保持代码整洁也更容易。

## 微服务的缺点

*   **微服务与单片应用相比，会产生不同类型的复杂性:**在单片中，模块之间的通信是通过同步函数调用来完成的，而在微服务中，您将必须实现某种通信，即 API 调用。你拥有的服务越多，你的交流就变得越复杂。调试也更加困难，每个服务都有自己的日志，您可能需要交叉引用这些日志来找到问题的根源。
*   **接口控制很关键:**每个服务都有自己的 API，其他服务依赖它来保持一致。如果您碰巧更改了 API，如果您没有使您的 API 向后兼容(在您的 API 中使用版本控制可以解决这个问题)，那么使用该接口的任何服务都将受到影响。

## 微服务应该有多大？

您确信微服务是您应用的发展方向，现在您想知道，我的微服务应该有多大？这是一个还没有人回答的大问题。这类事情没有经验法则，但一个好的衡量标准是:**一个微服务，一个责任**。但是正如 Martin Fowler 所说，如果你考虑工资，理论上它是一个责任，但是这会导致一个非常大的系统。也许最好的例子是两个比萨饼规则，负责一个微服务的团队应该只限于你可以用两个比萨饼养活的人数。所以，雇佣一些吃得多的人，或者如果你有预算，雇佣很多吃得少的人。

## 迁移到微服务

首先也是最重要的一点，不要一蹴而就！如果你决定微服务是你的企业要走的路，你应该一点一点去做。如果在应用程序的下一次迭代中，所有东西都被分离到微服务中，并且在部署之后，您发现没有任何东西像想象的那样工作，这将是非常糟糕的。您的迁移应该在每个迭代中分阶段进行，一砖一瓦地移除，直到您完全使用微服务。

**开始这个过程的一些好的提示是:**

*   查看您的应用程序，并尝试识别可以分离的业务逻辑。
*   找到自然隔离的代码，比如静态辅助函数。
*   通过与应用程序的其余部分分开进行扩展，寻找应用程序中可以受益匪浅的部分。这可能会节省成本，因为您将只扩展逻辑的一个部分，而不是整个应用程序(这会有更大的计算需求)。

# 结论

微服务可以是一种非常强大的架构，但正如一句流行的谚语所说，“权力越大，责任越大”，如果你选择使用它，就必须正确地使用它。如果做得好，你会看到一个新的可扩展的世界，但如果做得不好，你会陷入一个痛苦的世界，这可能会导致你回到巨石，浪费时间和资源，否则这些资源可以用于继续建造巨石。同样重要的是要注意，你不应该只是使用微服务，这样你就可以四处宣传，展示你的公司有多好，因为你正在使用微服务，重要的是要知道，整体并不是天生就不好，在大多数情况下，你只需将你的应用程序构建为整体，它将比使用微服务实现得更快，工作得更好。总是为你的任务挑选合适的工具。

我希望你喜欢看我的文章，如果你有任何建议或不同意我的任何观点，请让我知道。我并不自称是这方面的专家，所以非常欢迎您的反馈。

下次见。