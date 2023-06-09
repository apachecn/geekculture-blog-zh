# 颤振:软件包的力量——第一部分

> 原文：<https://medium.com/geekculture/flutter-the-power-of-software-package-part-i-ea5c6e145ca8?source=collection_archive---------35----------------------->

![](img/266a557b3cd05633389359c67b37fc7a.png)

Photo by [Bonneval Sebastien](https://unsplash.com/@gentlestache?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

正如它最初的名字一样，Flutter 或 Sky 是 Google 的跨平台开发的 SDK，不仅仅是移动，而是跨所有平台和设备的智能手机，平板电脑，网络，桌面，mac…

Flutter 于 2017 年首次发布，此后受到越来越多的关注，社区也越来越大

在本文中，我们不会讨论关于 flutter 的技术方面或如何构建一些东西，而是我们将关注 Flutter 如何在可伸缩性和软化方面将开发提升到一个新的水平。我们还将讨论 Flutter 如何成为寻求增长的公司的好选择

# 1-开发中的“软件包”

对于生产 B2B 解决方案的公司来说，获得新客户和保持老客户是公司最重要的事情，所以对他们来说，开发时间不应该是一个障碍，相反，他们需要用最短的开发时间提供有吸引力的报价

这里的解决方案是可配置性，即以一种可以快速重新编程的方式构建第一个系统(web 应用程序、移动应用程序、后端、API……)，并根据他们的需要重新生成另一个类似的系统，而不会损失与第一次相同的时间，这样他们就可以尽快获得新客户。

但是对开发者来说软件包并不容易，需要经验来有效地完成它。

例如，在移动应用程序中，可配置性可以具体化为应用程序主题、UI、功能、应用程序语言、API 端点，但应用程序核心需要相同(例如计算、模型、数据库、应用程序架构)

软件包最常见的例子是 ERP

# 2-对公司的好处

那么对企业有什么好处呢？

![](img/a080641eefc6a7f4b50bb0cda1941acd.png)

当然是钱！

获得新客户，意味着新的销售和成交，这就是生意

除了金钱，可编程应用在与其他公司的竞争中也非常有益，因此他们可以在游戏中领先，其他公司可以在几个月内建立的东西他们可以在几周内建立。

软件包的其他好处是，一旦他们有了一个稳定的系统，高效的调整系统，他们就不必雇用人才或经验丰富的高级开发人员，初级开发人员可以调整一些功能和主题，以生成新的应用程序。

但是他们不得不面对一些困难

为了创建一个高效的可配置系统，他们需要一个高级开发人员团队来完成这项工作，不仅如此，这个团队还需要全心投入到这个项目中。

除此之外，他们还为新客户开发新项目赢得了大量时间，第一个项目将比普通项目花费更多时间，因为它是建立一个软件包

# 3-移动应用如何可编程

使用 flutter 构建应用程序可以尽可能快，这是构建可编程移动/网络应用程序的主要因素。

我在这里试图与你分享的想法不是唯一的，有很多方法可以做到这一点，你可以选择其中的一种。

构建可编程项目的想法在于创建一个私有框架，包含所有可以使用的小部件，然后在将这个框架集成到您的项目中之后，您可以使用任何您想要的小部件。

因此，小部件将只构建一次，并集成到任意多个项目中，并且该框架的任何维护或更新都将适用于所有项目。

此外，如果所有的项目共享一些共同的业务逻辑，它可以在一个框架中开发和集成。

这些框架可以托管在 [pub.dev](https://pub.dev/) 上，也可以托管在 Github/Gitlab 上的私有存储库上。

通过这种方式，开发新项目将变得更加容易，也不需要任何高级技能，但所有高级资源将维护和创建框架，而初级开发人员将创建应用程序，就像一个拼图！

![](img/034a15bc2374b478c90d1962892bacbb.png)

正如我之前所说的，这是众多方法中的一种，你可以找到其他的解决方案，这取决于你选择哪种方法更适合你的情况。

# 4-结论

在这一部分中，我们讨论了什么是可编程应用，它们如何为公司和开发者带来好处，我们还看到了一种可用于创建可编程应用的方法。

我希望你喜欢这篇文章，并且学到了新的信息，在第 2 部分中，我们将使用这种方法实现一个简单的应用程序。

谢谢你给我时间！