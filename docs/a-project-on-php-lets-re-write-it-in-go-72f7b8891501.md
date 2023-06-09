# 一个关于 PHP 的项目？我们用 Go 重写吧！

> 原文：<https://medium.com/geekculture/a-project-on-php-lets-re-write-it-in-go-72f7b8891501?source=collection_archive---------14----------------------->

![](img/1067a981fe5fbdddce3fa6ff26c069f6.png)

作为一个企业主不要这样做。就是不要。

早在 NodeJs 发展势头越来越猛的时候，许多人就开始用它来进行后端开发，而不是像 PHP、Python 甚至 Perl 这样的 Web 主力。更糟糕的是，一些开发团队以某种方式说服高层管理人员使用新的流行技术重写现有的代码库。很快，他们中的许多人意识到，在编写复杂和交织的业务逻辑时，拥有回调地狱(Javascript 的固有特性)和古怪的基于原型的继承(需要相当长的时间来适应)并不是一件令人愉快的事情。

不要误解我的意思，在很多情况下，NodeJs 是一个完美的选择，但是现在我谈论的是业务逻辑。不言而喻，任何互联网业务都与数据和处理数据的规则有关。这些规则(或业务逻辑)有一些共同的特征:

首先，它们像我们的生活一样千变万化。今天，一个产品负责人想要一个特性，你，作为开发人员，跳到这个特性上，几天或者几周就完成了。你为你的工作感到骄傲。但是产品负责人来了，他说:“*我们最近一直在思考这个新功能，并决定在推出之前做一些改变，如果……*”。这里列出了您必须合并到已实现特性中的更改。您是否能及时成功地做到这一点，取决于您的代码在合并这些更改时有多灵活。有时，加入新的更改会对现有的实现产生雪崩效应，以至于从头开始重新编写要比试图让 ping 和大象交配容易得多。

第二，业务逻辑经常与许多例外和其他魔法相矛盾。像“**提出这个想法的人已经离开公司**”这样的情况也不少见，开发人员不得不直接从源代码而不是从技术文档中理解系统的行为。此外，业务逻辑通常是复杂而庞大的。当然，就其复杂性和美观性而言，它不是快速傅立叶变换算法，但是从源代码中理解大量的业务规则可能需要很大的努力。

第三，大多数情况下的业务逻辑是事务性的，属于请求-响应通信模型。客户可以执行的特定操作是以循序渐进的方式描述的。它要么成功，要么失败，并给出相应的响应。在大多数情况下，它不需要并发或并行执行模型。

那么，考虑到前面提到的几点，什么样的编程语言——低级还是高级——非常适合处理糟糕的文档、不断变化的需求、大量的特别解决方案和软件复杂性的情况呢？最肯定的是这是一个高层次的问题。与汇编语言相比，C 语言可以被认为是一种高级语言。你曾经试图阅读和理解用汇编语言编写的程序的源代码和用 C 语言编写的相同程序的源代码吗？这才是重点。高级语言表达能力更强。当处理业务逻辑时，与低级的相比，它们给你更好的理解和灵活性。

我在开始时说过，NodeJs 虽然是一种高级编程语言，但由于其古怪的基于原型的继承，以及它是作为一种浏览器语言在事件驱动的异步运行时环境中处理客户端 UI 逻辑而创建的事实，它对于后端开发来说是一种糟糕的选择。

那围棋呢？现在，我看到了这个行业曾经在 NodeJs 身上经历过的相同趋势。但是正如 NodeJs 过去和现在都不是实现业务逻辑的好选择一样，Go 也是。我来解释一下原因。

Go 是一种不支持面向对象编程的低级编程语言。我知道，某种 OOP 可以通过组合(即结构嵌入)来实现，但它仍然不能让你达到由高级语言提供的经典的基于类的 OOP 所能达到的抽象水平。而 OOP 反过来又是一个强大的范例，有助于应对不断增长的软件复杂性。对于后端开发来说，没有对 OOP 的全面支持是一个糟糕的选择。

一些 Go 的支持者可能会说:作为一种编译语言，Go 非常高效，在性能上可以击败任何动态语言，无论是 PHP、Ruby 还是 Python。这是真的，但有一个例外:大多数互联网企业根本不关心性能。他们主要关心的是新产品功能多快能交付给最终用户，也就是说，他们主要关心的是产品开发速度。

此外，一个熟练的开发人员是昂贵的，特别是如果我们谈论 Go 开发人员是目前收入最高的开发人员之一。另一方面，硬件和云解决方案很便宜。Go 带来的性能提升并不值得你的企业因为开发速度慢和支付给 Go 开发者的薪水而失去的东西。水平扩展通常会比用像 Go 这样的高效编译语言编写应用程序来压榨应用程序的性能更划算。

与 Web 开发中使用的其他编程语言相比，Go 有自己的优势，其中最重要的是:性能及其并发执行模型。它的 FFI (cgo)允许它粘合大量用无处不在的 C 语言编写的库，这使得它非常适合于跨平台命令行应用程序的开发。有了 gRPC 和 Go concurrency 这样的框架，它是实现各种网络相关服务的完美选择:图像大小调整、视频代码转换、键值数据库等等。任何需要执行的性能和并发模型的地方。正如我所说的，早期的商业逻辑与此无关。

总的来说，企业主应该知道开发人员喜欢玩新技术，有时，他们太急于在不符合其目的的地方使用新技术。如果你想你的生意成功，你应该意识到这一点。

对于 Web 开发者来说，不要再用显微镜敲钉子了。让我们去做自己擅长的工作，把剩下的留给守旧派:PHP、Ruby、Python 和 Java。