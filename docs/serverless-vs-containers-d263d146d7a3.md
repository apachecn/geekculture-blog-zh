# 无服务器与容器

> 原文：<https://medium.com/geekculture/serverless-vs-containers-d263d146d7a3?source=collection_archive---------10----------------------->

## 您应该使用哪种计算类型？

无服务器架构和容器架构在开发人员中非常受欢迎，它们都有各自的优点和缺点。当开发人员决定如何构建和托管他们的下一个项目时，理解它们之间的关键区别并知道何时使用哪个选项是很重要的。

传统上，人们使用虚拟机和服务器来托管他们的项目。老实说，如果我需要做一些一次性测试，启动 EC2 实例并在之后终止它可能更容易。然而，如果我们考虑可持续性和开销，容器和无服务器计算是比虚拟机更轻量级的选项，但仍然是可扩展的，如果不是更多的话。

# 什么是无服务器计算？

无服务器计算是一种开发人员无需维护任何服务器或后端基础设施的方法。所有计算资源都由云提供商在幕后管理和维护。开发者只需要专注于函数本身的编码和开发。

然后，基于某些指标向开发人员收费，例如提供的内存、函数运行的总时间、函数运行的次数等。

# 什么是容器？

容器是包含应用程序和应用程序正常运行所需的元素的组件，例如系统库、配置和依赖项。容器允许将应用程序打包并从底层基础设施中分离出来，然后可以将其部署到另一个环境中。

容器利用主机的操作系统内核，不需要像虚拟机一样运行整个操作系统。容器有助于分解和分离应用程序的不同组件，例如为 web 服务器、应用服务器等提供一个容器。

您将应用程序打包成映像，并将这些容器映像部署到容器应用程序中。默认情况下，每个容器都是无状态的，一旦终止，就不会保留任何数据。

# 无服务器计算与容器

## 类似

*   只要不做任何更改，每次都一致地部署代码
*   与虚拟机相比，所需的开销和维护更少
*   需要时可自动扩展

## 费用

对于大多数以商业为导向的人来说最重要的因素。容器的好处是它们大多数都是开源的，比如 Kubernetes，这意味着你可以在一个环境中运行它们，而不需要支付任何许可。但是，您将为底层基础设施付费。这意味着成本可能很高，因为容器总是在运行，即使应用程序当时没有使用。

在无服务器架构中，只根据功能运行的持续时间和频率以及其他指标(如提供的内存)对您的使用进行计费。无服务器赢得了这个类别，因为你只需为你使用的资源付费。

## 局部环境

使用 Docker 和 Amazon Elastic Container Registry 这样的工具，可以很容易地在云和本地环境中互换使用容器。它就像加载映像并使用该映像部署容器一样简单。

对于无服务器功能，可能会稍微复杂一点。它们只存在于云中，但一些工具和框架，如 AWS 无服务器应用程序模型(SAM)，允许您在本地部署和测试无服务器功能。

## 维护

容器及其配置需要由开发人员管理、构建和更新，而无服务器功能只需要开发人员维护其代码，而不需要维护后端基础设施。

## 有效性

容器能够长时间运行，并且能够处理等待时间长或持续时间长的工作负载。然而，无服务器功能通常使用很短一段时间，一旦它们处理特定的事件或功能就会终止。

无服务器功能服务通常有最大持续时间限制，比如 AWS Lambda 为 15 分钟，Azure 功能为 5 分钟。

## 部署速度

容器的启动往往比虚拟机快得多。虚拟机可以在几分钟内完成部署，而容器可以在几秒钟内完成部署。然而，无服务器功能能够在几毫秒内部署。

这是因为容器需要更多的设置时间来配置设置、安装库和依赖项等等。

# 两者之间该如何选择？

选择哪个选项实际上取决于应用程序的需求。

## 选择无服务器

*   快速发布和迭代您的应用程序代码
*   只需要代码运行一小段时间
*   功能没有定期运行，并且不会经常处于高使用率状态
*   您不想担心后端基础架构的管理
*   您希望根据需要自动扩展应用程序
*   您希望轻松、经济地设置您的功能

## 选择容器

*   您需要对您的环境和依赖关系有更多的控制
*   您需要安装库或运行长时间的工作负载
*   你需要你的容器在不使用的时候也能运行
*   您需要迁移一个遗留应用程序
*   您希望利用容器编排工具
*   您想要更多的灵活性和可配置的元素

# 两者并用

如果您有一个复杂或大型的应用程序，您可能会发现同时使用无服务器计算和容器实际上会更有好处，因为它们针对您的特定用例弥补了彼此的不足。

鉴于云的易用性，您实际上不需要只采用一种方法。您可以轻松地将它们集成在一起，以获得最大的收益。

这完全取决于你需要完成什么，以及哪个选项有助于你实现目标。