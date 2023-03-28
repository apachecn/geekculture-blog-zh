# 借助强大的数据库提高移动应用性能

> 原文：<https://medium.com/geekculture/improving-mobile-app-performance-with-a-powerful-database-5e33846422cb?source=collection_archive---------21----------------------->

![](img/f4767beb4091ac6af002ab2ac57b62c9.png)

您可能意识到，在为您的移动应用程序选择数据库和其他技术时，似乎有无穷无尽的选择可以考虑。面对如此多的选择，当涉及到您的技术堆栈时，很难确定什么才是真正重要的，这可能会令人困惑。不久前，我写了一篇关于不同的[数据库架构和用例](https://www.harperdb.io/post/database-architectures-use-cases)的文章，为正确的项目选择正确的技术提供指导。虽然这仍然是一个准确可靠的资源，但本文深入探讨了提高特定移动应用程序性能的注意事项。

# 移动应用与网络应用

首先，也许我们应该快速了解一下移动应用和网络应用之间的区别。移动应用程序在移动设备上运行，而网络应用程序是通过网络浏览器访问的，可以适应你在任何设备上查看它们。[原生移动应用](https://careerfoundry.com/en/blog/web-development/what-is-the-difference-between-a-mobile-app-and-a-web-app/)是为特定平台打造的，比如苹果的 iOS 或安卓，嗯，几乎所有其他平台都是如此。它们通过应用商店下载和安装，可以访问系统资源，如 GPS 和相机功能。[然而，网络应用](https://careerfoundry.com/en/blog/web-development/what-is-the-difference-between-a-mobile-app-and-a-web-app/)并不是特定系统的原生应用，不需要下载或安装。由于它们的响应特性，它们的外观和功能可能很像移动应用程序，这也是一些困惑产生的地方。

为了更深入地挖掘，使用特定于平台的 SDK 为目标平台创建的移动应用被分类为[原生移动应用](https://www.clariontech.com/blog/mobile-app-vs-web-app-which-is-the-right-one-for-you)。鉴于[混合移动应用](https://www.clariontech.com/blog/mobile-app-vs-web-app-which-is-the-right-one-for-you)在提供与所有可用操作系统兼容的代码的平台上开发。最后，你可能听说过渐进式网络应用(PWA ),许多人声称这是未来。PWA 的“重点是创建外观和感觉完全像本地应用程序的 web 应用程序，用户不必下载和安装任何软件。”

# 移动应用性能挑战

虽然许多公司可能拥有令人难以置信的技术，但他们的移动应用程序经常面临性能、延迟和/或连接问题。这可能是由多种因素造成的。也许该组织没有投入大量时间或资源从头开始构建他们的移动应用程序，因此他们没有一个坚实的基础。或者，他们可能正在努力解决由于[集中式数据库](https://www.harperdb.io/post/reduce-latency-geo-distributed-databases)和云/供应商锁定带来的延迟问题。说到数据库，它是否能够处理适当数量的用户和频繁的更新？它是离线存储数据还是处理复杂的查询？这些都是从头开始构建一个新的移动应用程序，或者向现有应用程序添加新功能等时需要考虑的事情。请记住，如果需要的话，从一个数据库迁移到另一个数据库并不是不可能的(实际上，对于某些技术来说，这是非常容易的)。

# 数据库注意事项

在我的[数据库架构和用例](https://www.harperdb.io/post/database-architectures-use-cases)博客中，我提到在选择数据库时，考虑您的数据类型/结构、数据量、一致性、写&读频率、托管、成本、安全性和集成约束很重要。虽然这些肯定是正确的，但在移动应用程序方面，您可能还需要考虑其他一些事情:

*   支持多种移动应用平台
*   可量测性
*   数据同步
*   多层数据模型考虑事项
*   网络连接
*   推送新的应用程序更新和数据库更改
*   解决设备之间的数据冲突

很明显，这里有很多需要考虑的事情，并且很难确定哪种类型的数据库技术可能是最适合的。首先，就数据结构/功能类型而言，一些数据库非常适合一个类别。其他技术更多地采用混合方法，支持交叉功能或将不同工具的功能组合成一个工具。因此，除非您有一个非常具体的项目或有限的长期目标，否则最好采用更灵活的混合技术，在一个包中包含更多功能，以减少所需的系统数量。

# 那么，应该使用哪个数据库呢？

在这里，我将尝试提供一个公平的(虽然可能仍然有点偏颇)解释，为什么 [HarperDB](https://harperdb.io/?utm_source=medium) 是提高你的移动应用性能的最佳选择。从高层次来看，HarperDB 是一个现代的混合数据库，它将市场上一些最好的工具的功能结合在一起，因此它确实涵盖了许多基础。作为一个分布式数据库，它可以安装在任何地方，同时呈现一个跨云范围的单一界面，并具有后端能力来保持数据在任何地方同步。HarperDB 经过读写优化，每个节点每秒可处理 100K 以上的请求。

当然，还有其他很好的选择。例如，[几年前发表的这篇文章](https://www.simform.com/mobile-app-developers-database-selection/#criteria)根据所需的功能列出了不同的数据库选项。HarperDB 基本上适用于该表中的任何地方，并提供所有提到的功能。

不要试图比较市场上 300 种不同的数据库选项，这可能会简化您进行这种与那种比较的决策。这将使您能够更深入地了解自己的需求，并比较性能和成本等因素。例如，在[基准测试](https://harperdb.io/harperdb-vs-mongodb/?utm_source=medium)中，HarperDB 比 MongoDB 快 37 倍，并且更具成本效益。它还支持 JSON 上的 SQL。蟑螂更适合金融科技用例，而 HarperDB 可能更适合游戏、票务、军事和媒体等行业。您不能全局锁定 HarperDB 的数据库，我们的集群方法依赖于最终的一致性，这允许比更结构化的选项更高效的复制。像 MySQL 这样的关系数据库需要更多的资源，需要更多的维护和严格的数据结构。而 HarperDB 可以在从 Raspberry Pi 到超级计算机的所有垂直规模的机器上运行，几乎不需要维护，并且具有允许轻松获取数据的动态模式。这个[Harper db vs . MongoDB vs . PostgreSQL](https://www.harperdb.io/post/harperdb-vs-mongodb-vs-postgresql)的比较可能也有帮助。

您是否面临应用程序[延迟](https://www.harperdb.io/post/reduce-latency-geo-distributed-databases)方面的挑战？还是担心被真正的[地理分布](https://dev.to/harperdb/industries-that-need-a-high-performing-low-latency-distributed-database-5cn4)？有了 HarperDB，您只需旋转更多节点进行水平扩展，将 HarperDB 放在更靠近最终用户的各个区域，这将减少延迟并提高应用性能，同时实时访问数据。通过分发 API 和数据存储，并将应用程序逻辑转移到边缘，您可以消除瓶颈，减少基础架构和成本。HarperDB 以互联网的速度在全球复制数据，减少了应用程序延迟，提高了性能和可访问性，并降低了数据管理的整体复杂性。最后，HarperDB 最近发布了[自定义函数](https://harperdb.io/docs/custom-functions/)，这使得开发人员能够利用核心 HarperDB 方法编写自己的自定义 API 端点，从而简化技术堆栈并提高性能。

移动应用程序在不断发展，您需要一个灵活的数据库，能够在不影响性能的情况下进行动态调整。即使是世界上最先进、最令人印象深刻的技术，也会被基础差或过时的数据库拖垮。跟上现代技术是在这个竞争激烈的市场中生存的最佳方式。因此，当涉及到您的技术组合时，不应该掉以轻心。