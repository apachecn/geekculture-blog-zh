# 分布式计算的八大谬误

> 原文：<https://medium.com/geekculture/the-eight-fallacies-of-distributed-computing-44d766345ddb?source=collection_archive---------6----------------------->

## 多亏了现代计算，分布式计算的 8 个谬误正在变得过时

![](img/19c10ed09d0a35365af3fd25497a15f1.png)

Photo by [Alexandre Lion](https://unsplash.com/@alxlion?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

二十多年前，彼得·多伊奇和詹姆斯·高斯林定义了分布式计算的八大谬误。这些是许多开发人员对分布式系统做出的错误假设。从长远来看，这些通常被证明是错误的，使得修复错误变得困难。那么，让我们来详细看看那八个谬误是什么。

## 谬论一:网络可靠

网络从来都不可靠。当它们通过线路传输时，会有数据包丢失、连接中断和数据损坏。此外，网络中断、路由器重启和交换机故障使事情变得更糟。如果硬件还不够，软件也可能会失败，而且确实如此。如果您与外部合作伙伴协作，情况会更复杂，例如与外部信用卡处理服务合作的电子商务应用程序。他们这边的连接不在你的直接控制之下。在设计健壮的分布式系统时，必须考虑这种不可靠的网络。让我们举一个简单的例子:

```
var creditCardProcessor = new CreditCardPaymentService();
creditCardProcessor.Charge(chargeRequest;
```

如果我们收到一个 HTTP 超时异常会发生什么？如果服务器没有处理请求，那么我们可以重试。但是，如果它确实处理了请求，我们需要确保我们没有向客户重复收费。您可以通过使服务器幂等来做到这一点。这意味着，如果您以相同的收费请求呼叫它 10 次，客户将只被收费一次。如果你不能正确处理这些错误，你的系统就是不确定的。处理所有这些案件会变得非常复杂。

这对你的设计意味着什么？这是否意味着我们需要放弃当前的技术体系，转而使用消息传递系统？大概不会！我们需要权衡失败的风险和你需要做的投资。您可以通过投资基础设施和软件来最大限度地降低失败的几率。在很多情况下，失败是一种选择。但是在设计分布式系统时，您确实需要考虑失败。总而言之，网络是不可靠的，作为软件架构师/设计师，您需要解决这个问题。

## 谬误 2:延迟为零

互联网上的通话不是即时的。延迟是指数据从一个地方移动到另一个地方所需的时间(相对于带宽，即在此期间我们可以传输多少数据)。在局域网上，延迟可能相对较好，但当您转向广域网或互联网场景时，延迟会迅速恶化。网络延迟是真实存在的，我们不应该假设所有事情都是瞬间发生的。通过跨大西洋通信电缆传输数据非常困难，并且会增加延迟。

*   带回你可能需要的所有数据:如果你进行远程呼叫，确保带回你可能需要的所有数据。网络交流不应该是闲聊。
*   将数据移动到离客户机更近的地方:另一个可能的解决方案是将数据移动到离客户机更近的地方。如果您使用的是云，请根据您客户的位置仔细选择可用区域。缓存还有助于最小化网络调用的数量。对于静态内容，**内容交付网络(CDNs)** 是另一个不错的选择。
*   反转数据流:移除远程调用的另一个选择是反转数据流。我们可以使用发布/订阅模型并将数据存储在本地，而不是查询其他服务。这样，当我们需要数据时，我们就有了数据。当然，这引入了一些复杂性，但它可能是工具箱中的一个好工具。

## 谬误 3:带宽是无限的

带宽不是无限的。无论是机器，还是服务器，或者是进行通信的线路。虽然带宽随着时间的推移有所提高，但我们发送的数据量也在增加。VoIP、视频和 IPTV 是占用带宽的一些较新的应用。下载、更丰富的用户界面以及对冗长格式(XML)的依赖也增加了负载。

第二个谬误，**延迟不为 0** ，第三个谬误，**带宽无限大**之间存在张力。您应该传输更多的数据，以最大限度地减少网络往返次数。您应该传输较少的数据，以最大限度地减少带宽使用。您需要平衡这两种力量，并找到通过网络发送的正确的数据量。

虽然您可能不会经常遇到带宽限制，但是考虑您传输的数据是很重要的。数据越少越好理解。更少的数据意味着更少的耦合。所以只传输我们可能需要的数据。

## 谬误 4:网络是安全的

没有人会天真到认为是这样。许多恶意用户不断试图嗅探网络上的每个数据包，并对正在通信的内容进行解码。因此，请确保您的数据经过加密，并尽可能降低安全风险。

*   深度防御:您应该使用分层方法来保护您的系统。您需要在网络、基础设施和应用程序级别进行不同的安全检查。
*   安全性思维:在设计系统时，请牢记安全性。[十大漏洞](https://www.owasp.org/index.php/Category:OWASP_Top_Ten_Project)列表在过去 5 年中没有太大变化。您应该遵循安全软件设计的最佳实践，并检查代码中常见的安全缺陷。您应该定期搜索第三方库中的新漏洞。常见漏洞和暴露列表会有所帮助。
*   威胁建模:威胁建模是识别系统中可能的安全威胁的系统方法。您首先识别系统中的所有资产(数据库中的用户数据、文件等)以及它们是如何被访问的。之后，您识别可能的攻击并开始执行它们。

事实是，安全很难，而且很昂贵。分布式系统中有许多组件和链接，它们中的每一个都可能成为恶意用户的目标。

## 谬误 5:拓扑不会改变

网络拓扑一直在变化。有时它会因为意外的原因而改变——当我们的应用服务器出现故障，我们需要更换它时。但大多数情况下，这是有意的——在新的服务器上添加新的进程。如今，随着云和容器的兴起，这一点更加明显。弹性扩展—根据工作负载添加或删除服务器的能力—需要一定程度的网络灵活性。

当拓扑发生变化时，我们可能会看到延迟和包传输时间的突然偏离。因此，需要监控这些指标的任何异常行为，我们的系统应该准备好迎接这种变化。

*   停止硬编码 IP——我们应该更喜欢使用主机名。通过使用 URIs，我们依靠 DNS 将主机名解析为 IP。
*   当 DNS 不够用时(例如，当我们需要映射 IP 和端口时)，则使用发现服务。
*   服务总线框架也可以提供位置透明性。
*   任何服务器都可能发生故障(从而改变拓扑)，所以我们应该尽可能地自动化。
*   停止服务或关闭服务器，查看系统是否仍在运行。像网飞的[混沌猴](https://github.com/Netflix/chaosmonkey)这样的工具通过随机关闭生产环境中的虚拟机或容器，将这一点提升了一个档次。通过将痛苦提前，我们更有动力构建一个更有弹性的系统来处理拓扑变化。

## 谬误 6:只有一个管理员

过去，通常由一个人负责维护环境、安装和升级应用程序等。然而，随着向现代云架构和开发运维实践的转变，这种方法已经发生了变化。

现代的云原生应用由许多服务组成，它们协同工作，但由不同的团队开发。一个人几乎不可能知道和理解整个应用程序，更不用说试图解决所有问题了。

实施治理，使出现的任何问题的故障排除变得容易。发布管理、解耦、日志和监控等概念在这里都适用。

## 谬误 7:运输成本为零

这个谬误与第二个谬误有关，即**潜伏期为零**。在网络上传输东西是有代价的，无论是时间还是资源。如果第二个谬论讨论了时间方面，谬论#7 解决了资源消耗。在使用分布式系统时，我们都要承担硬件、软件和维护的隐性成本。例如，如果我们使用类似公共云的 AWS，那么数据传输成本是真实的。从鸟瞰图来看，这一成本几乎为零，但在大规模运营时，这一成本变得非常重要。此外，对象序列化和反序列化也是有代价的。就性能而言，这两种操作的成本都很高。

我们应该注意传输成本以及应用程序正在进行多少序列化和反序列化。这并不意味着我们应该优化，除非有需要。我们应该衡量和监控资源消耗，并决定运输成本是否是我们的问题。

## 谬误 8:网络是同质的。

网络不是同质或同类的。相反，网络是异构的。同构网络是使用相似配置和相同通信协议的计算机网络。拥有相似配置的计算机是一项很难实现的任务。还有，你不能假设网络硬件总是一成不变的。例如，您无法控制哪些移动设备可以连接到您的应用程序。您应该选择标准格式，以避免供应商锁定。这可能意味着 XML、JSON 或协议缓冲区。有很多选项可供选择。关键点是关注标准协议，以便组件可以通信，而不管硬件如何。

在这篇文章中，我们了解了分布式系统的谬误以及如何避免它们。我们必须接受这些事实:网络不可靠，不安全，而且要花钱。带宽有限。网络的拓扑将会改变。其组件的配置方式不同。意识到这些限制将有助于我们设计更健壮、更安全的分布式系统。想了解更多关于分布式系统的知识吗？这里有我写的另一篇关于[分布式系统](/codex/introduction-to-distributed-systems-66502ac8289)的文章的链接。看一看！

*参考文献:*

[](https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing) [## 分布式计算的谬误-维基百科

### 分布式计算的谬误是太阳微系统公司的 L·彼得·多伊奇和其他人提出的一组断言…

en.wikipedia.org](https://en.wikipedia.org/wiki/Fallacies_of_distributed_computing)