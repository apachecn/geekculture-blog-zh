# 为什么我过度工程化。

> 原文：<https://medium.com/geekculture/why-i-over-engineer-c251beab5a04?source=collection_archive---------13----------------------->

或者为什么我不应该…

![](img/1b755625be809f141b4baa922b753ad0.png)

Photo by [Deva Darshan](https://unsplash.com/@darshan394?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我注意到在这个问题上至少有两个思想流派，我认为回顾并看看辩论双方的利弊可能是一个有趣的话题。

因为我喜欢首先从更有说服力的一面开始，所以我将从讨论我认为过度工程的最大缺点开始。

# 后退

## 这将花费 X(时间、金钱、才能)

![](img/d5c23e847ce53489eab7055dd5486e57.png)

Photo by [Jp Valery](https://unsplash.com/@jpvalery?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

可能最明显的一点是，大多数时候你是被别人付钱来写代码的。除非你是根据不同的协议被雇佣的，否则大多数时候你都是被雇佣来编写与创造有价值的产品最直接相关的代码。这通常包括编写尽可能少的代码，有足够的测试覆盖率来满足(希望)良好定义的软件需求规格，这样您就可以尽可能有效地转移到下一个需求。

现在，每家公司对代码质量和项目约束(时间、金钱等)都有不同的价值观和信念，但在大多数情况下，可以肯定地说，你是通过生产产品获得报酬的。

## 没有直接增值

![](img/e04387ec5d07eeed61361142f9466918.png)

Photo by [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这一点本身是有争议的，但本质上，如果您编写的代码比完成一项任务所需的代码多，而这项任务原本可以用一半的时间完成，那么花在创建一个奇妙抽象上的所有额外时间现在都被“浪费”在产品的下一个可交付特性上了。

现在，在这一点上，我将停止在这里，因为它是安全的说，任何进一步的点也将以某种方式与时间，金钱或人才的花费有关。所以让我们继续讨论过度设计的好处。

# 赞成的意见

我将在这一节开始说，随着这些过度工程化的有利原因而来的是一种平衡，但是我将在总结中更深入地讨论这一点。

## 清除器代码

![](img/31d63811b52cc3ecaa89ef3b1ac439d7.png)

Photo by [Edgar Chaparro](https://unsplash.com/@echaparro?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果您能够花费足够的开销时间来确保您为实现选择的设计提供了正确类型的架构和抽象，那么对代码库进行推理会容易得多。

对我个人来说，它与拥有一个干净的工作空间/办公桌区域有一定的关联，在某种意义上，如果你尊重自己，你也应该尊重你生活和工作的区域(包括代码库)。这也可以为新团队成员进入您的系统提供一个更容易的障碍。

## 开发者体验

![](img/47cb3bf9d3cbd51a40339eb9837f0dd4.png)

Photo by [Pascal Habermann](https://unsplash.com/@pascal_habermann?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

获得对事物的真正理解的最简单的方法之一就是投入进去并体验它。

这适用于代码设计和实现，因为当你着手改进或设计一个新系统时，你不得不做研究并找出如何做某事(或如何做得更好)。这是我职业生涯的基础，我有机会在实际代码中使用许多不同的软件设计范例和最佳实践。如果没有我的经历，我可能不会真正理解为什么对某些东西(如 web 服务器)使用库更好。在我早期的 node 项目中，我犯了一个错误，使用 tcp 模块在 node 中从头开始编写 API 服务器，并手动解析 http 消息。是的，这是一个非常愚蠢的想法，但现在我真正欣赏像 express 这样的模块。

有了正确的心态，你可以带着你所做的，批判性地回顾它，说，什么有用，什么没用，我下次要做得更好。

## 可重用性和可扩展性

![](img/5d4fa0ced2d61a98a26fdf3efcac3ce4.png)

Photo by [Jasmin Sessler](https://unsplash.com/@jasmin_sessler?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我把这个放在最后，因为在我的经历中，这是最让我痛苦的一个。

过度工程之所以有用的原因是，在未来，由于您之前创建的抽象/模块等，您可以用最少的努力实现另一个类似设计的功能。原因还在于，在不久的将来，我们可能会有巨大的流量激增或客户群增长，如果我们不提前优化，这将导致巨大的恐慌。

在我看来，这就是为什么在使用它并在代码编辑器中运行之前需要考虑和小心的原因。原因是，这有一个非常有效的理由，可能(或不太可能)在未来发生，但事情就是这样。这是未来…意味着你并不真正知道。我援引这个理由来创造某物的资格如下:

*   是否需要相对较少的时间来实现？
*   是否有足够的人可用，或者是否有足够的时间不能用来创造实际价值？
*   权衡的风险/概率是否足够高，以至于需要实施这一点？

如果这些都是真的，那么如果你也有适当的权利去做这件事，那么说向前迈进是公平的。

# 摘要

总的来说，我仍然发现很多有价值的理由来花时间设计一个有思想的和结构良好的系统，并且在适当的情况下可能倾向于这种偏见。然而，在某些情况下或在某些公司里，尽可能快地创造和发布一些东西会有更多的价值(比如说一个创业 MVP)。真正的答案取决于手头的具体情况和一些真正的批判性思考，以决定什么是最佳的前进道路。

既然我已经在这个话题上发表了我的想法…我很乐意听到任何其他人的想法/问题或对提到的任何观点的不同意见，或者对这个话题的任何想法，所以请在下面的评论中写下它们！