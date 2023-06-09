# 2022 年，Swift 将成为跨平台、通用编程语言

> 原文：<https://medium.com/geekculture/swift-as-a-cross-platform-general-purpose-programming-language-in-2022-d039f61b3a5c?source=collection_archive---------4----------------------->

## Swift 会成为统治它们的语言吗？

![](img/74fc8ce1bb917158ac3570b71c3c9caf.png)

Image rendered using [NightCafe’s](https://creator.nightcafe.studio) text-to-image AI using the phrase ‘ring inscribed with Swift’.

现代编程是复杂的。在我最近的一个移动应用 [BG Snapshot](https://www.dabblingbadger.com/software/ios/bg-snapshot) 中，我利用 shell 脚本完成低级文件系统任务，利用 Python 进行数据管理和机器学习(ML)，利用 C++进行近似最近邻搜索，利用 Swift 构建最终的 iOS 应用。这个过程和它的组织感觉笨拙、混乱，我的大脑不喜欢不断的上下文切换。在 Swift 中端到端地编写复杂的应用和工作流不是很好吗？今天，我想简单谈谈 Swift 当前的生态系统、一些我认为缺失的库和支持领域，以及 Swift 需要什么才能成为“统治一切的单一语言”的竞争者。

# 前言

作为程序员，我们是一群不拘一格的人。我们每个人都通过独特的视角看待编程领域，这种视角是由我们的培训、工作经验和专业领域形成的。我来自医学信息学和机器学习领域。我喜欢使用 Python 和 r 之类的脚本语言。目前我最喜欢的工作流程是一个文本编辑器，命令行上有一个读取-评估-打印循环(REPL)。当我想到我想从 get-stuff-done 语言中得到什么时，我经常发现自己在使用 Python 及其健壮的生态系统作为比较的模型。在我分享我对 Swift 生态系统的愿景和愿望清单时，请记住这一点。可能和你自己的不搭…也没关系:)

# 2022 年的 Swift 生态系统

在我看来，Swift 生态系统由 Swift 语言、标准和核心库(目前主要来自苹果)、专有库(也包括苹果，但也包括其他公司)、开源包和在线社区组成。在 2022 年，我们有很多重要的组成部分，所以让我们来回顾一下。

在撰写本文时，Swift 的版本是 5.6。我不打算浪费你的时间列出所有强大的特性(泛型、协议、可选的等等。)你已经开始了解和喜爱，但让我只说一两件事。Swift 的标准库和基础提供了开箱即用的*功能。事实上，随着最近 async await 的加入，它在语言层面上解决了并发性问题，我正在努力列出 Swift 仍然缺乏的主要特性。这是件好事。*

就跨平台能力而言，我们拥有对三大桌面平台的核心语言支持:MacOS、Linux 和 Windows。当然，也为苹果的其他产品提供了出色的支持，包括 pad、移动设备、手表和电视。从技术上讲，您可以在 Android 上编译和运行标准库，但是正如我稍后将讨论的那样，这里没有库支持，所以在这一点上您真的不能做太多。但是，您知道 Swift 也偷偷溜进了其他一些平台吗？一个名为 [SwiftWasm](https://swiftwasm.org) 的项目可以让你将 Swift 代码编译成 web 汇编，直接在浏览器中运行。他们的网站包括一个游乐场，该游乐场有一个图形用户界面，其 API 与 SwiftUI 相同。[来看看](https://pad.swiftwasm.org)！对于喜欢修补物联网/微控制器领域的人来说，你可以在 [Arduino](https://www.swiftforarduino.com) 上运行 Swift 的配对版本。虽然不像 C 或 Java 那样具有可移植性，但 Swift 很快就可以在广泛的平台上使用。

让我们以一些关于库和包管理的简要亮点来结束我们对 2022 年 Swift 生态系统的回顾。开源库和框架是语言的生命线。它们允许我们在更高的抽象层次上操作，将更多的资源用于解决新问题，并快速开发和部署最终产品。Swift Package Manager (SPM)提供了一项核心技术，促进了这一命脉的发展。它集成到构建系统中，预期的项目组织非常简单，包清单也是用 Swift 编写和消化的，非常棒。向豆荚和卡丁车说再见吧。我们离 Swift 的端到端开发又近了一步。

# 我们错过了什么

对于一门跨平台和工作领域无处不在的语言，我认为我们需要三样东西:

1.  真正的多平台支持，包括用于构建 UI 的工具。
2.  一个强大的开源软件包目录，包含用户友好的 API 和完整的文档
3.  一个充满活力的开发者社区提供教育(通过书籍、教程、博客等)。)、故障排除(通过 StackOverflow 之类的东西)以及启发其他语言的开发人员采用 Swift。

## 1.真正的多平台支持

首先，让我们解决房间里的大象。 ***苹果*** 。尽管 Swift 是开源的，但它首先是为达尔文设备开发的。事实上，我们可以在其他地方运行 Swift 代码是很棒的，但是苹果有什么动机在 Android、Windows 和其他非达尔文平台上提供支持呢？苹果公司通过他们销售的硬件和服务赚钱，我希望该公司为自己的底线服务。话虽如此，苹果显然对在其他平台上运行 Swift 的想法持开放态度……毕竟我们有 Unix 和 Windows 版本。Swift 社区只需接受大部分跨平台支持必须来自其用户群和会员。

在核心库和基础中还存在其他必须解决的挑战。Swift 完全是为了与 Objective-C 互操作而构建的。这样做是有意的，以便在我们采用 Swift 的同时，苹果和第三方开发者(美国)可以继续利用用 Objective-C 编写的代码库和项目。我不会假装理解所有的细节，但这也是许多跨平台问题的根源。Objective-C 运行时和基础库只在 Apple 平台上可用。其他所有平台的 Swift 使用不同的核心 C 库(苹果用 Darwin，Unix 用 Glibc，Windows 用 WinSDK？)并拥有独立版本的[基金会](https://github.com/apple/swift-corelibs-foundation)，其 API 与苹果基金会的 API 相匹配。至少可以说，在一个非常低的水平上，有一点混乱。非苹果平台的 Foundation 的实现是不完整的，尽管公平地说，有许多关键组件[似乎已经很好地覆盖了](https://github.com/apple/swift-corelibs-foundation/blob/main/Docs/Status.md)(运行时实体、原始数据类型、集合等)。).我希望比我聪明的人能解决这些问题，这样我们其他人就能编写可靠的跨平台工具。如果我不得不在基础中记忆平台差异或者写一堆编译器指令，这将是一个大麻烦。

Swift 缺少的另一个重要的跨平台基础设施是一套用于开发用户界面的工具。还记得我说过你可以编写编译成 web 程序集的 SwiftUI 代码，并在你的浏览器中运行它吗？这正是我们在 Swift 平台无关的 UI 框架中需要的东西。这让我想到了像 [GTK](https://www.gtk.org) 和 [Qt](https://www.qt.io) 这样的开发套件。一些新的和本土的东西将是理想的，但 GTK 的快速绑定至少会让我们启动和运行。

## 2.一个强大的开源软件包目录，包含用户友好的 API 和完整的文档

公平地说，Swift 中可用的绝大多数公共包都是面向苹果设备上的开发的。因此，它们中的大多数在其他地方都没有用，这是一个大问题。为了让 Swift 在其他平台上获得牵引力，我们需要一个强大的跨平台包基础设施以及一个高效的包发现系统。

其中一些构件正在建造中。有一个名为 [Swift 包裹索引](https://swiftpackageindex.com)的早期项目，作为 Swift 包裹的搜索引擎。该索引目前有超过 4，800 个包的元数据，包括关于项目状态、需求和兼容性的有用信息。Swift 包索引甚至提供 DocC 兼容文档的托管，这真的很酷。我希望看到项目发展到包含 restful API 和命令行工具，使安装和添加包到项目变得更加容易。手指交叉。

在 Swift 中开发一个强大的跨平台包生态系统需要时间。由于我们没有 GUI 框架，而且近期内也不会有，我认为专注于脚本、命令行库和底层实用程序将有助于推动其他领域的发展。Swift 中的脚本是我一直在探索的东西，有几个关键领域需要一些工作。在 Python 中，我可以轻松地导入包并调用包含在多个文件中的功能。对于 Swift 脚本来说，这些都不是小事。除了 Foundation 之外，导入其他库的最简单方法是使用 SPM，但是，您实际上不再编写脚本了…现在您正在创建一个包含可执行文件的小软件包。swift-sh 软件包可能会提供一个解决方案，但是我还没有将它添加到我的工作流程中。第二个问题，从多个文件调用代码，也有一个难看的解决方案。您可以使用`cat`命令提前连接多个 swift 文件，并将结果传输到对 Swift 的调用中。但同样，这感觉像是一个权宜之计，而不是一个长期的解决方案。Swift 中快速高效的脚本可以成为真正的游戏改变者，但是我现在坚持使用 Python。

命令行应用程序更多的是小众领域，但是我猜你们中的一些人和我一样是超级用户，并且在命令行上感觉非常自在。 [ArgumentParser](https://github.com/apple/swift-argument-parser) 包是一个受欢迎的附加包，还有一些其他包帮助应用程序开发，添加格式和颜色等。，但我认为我们可以使用更多。有没有一个可以从命令行启动的 Playgrounds 版本？我认为我们需要一些非苹果平台的东西。Swift 确实有一个 REPL，它通过使用 LLDB 来支持调试器，但我希望看到更像 iPython 的东西。如果你不熟悉， [iPython](https://ipython.org) 是 Python 的一个 REPL，它包括语法高亮显示、带有精美菜单的代码补全、查看输入历史的神奇命令，以及大量其他漂亮的东西。这是一个神奇的工具。我专门在 iPython 中工作，负责机器学习和数据管理任务，并且希望看到一个适用于 Swift 的类似工具！

位于 Foundation 等之上或旁边的较低级别的实用程序包可以为 Swift 的采用催生全新的领域。我们已经在 web 服务领域看到了一个这样的例子。 [Vapor](https://vapor.codes) 和 [Kitura](https://www.kitura.dev) 开启了一个全新的世界，Swift 正被用于服务网页、restful APIs 等等。实际上，苹果公司还有许多其他正在进行的项目，我会把它们包括在这个空间里……它们只是没有被很好地宣传。我在浏览 GitHub 后注意到的几个包括:[集合](https://github.com/apple/swift-collections)、[系统](https://github.com/apple/swift-system)、[算法](https://github.com/apple/swift-algorithms)、[原子](https://github.com/apple/swift-atomics)和[分发器](https://github.com/apple/swift-distributed-actors)。

就个人而言，我希望 Swift 成为数据科学家的有用语言。我并不完全孤独。Chris Latter 在谷歌工作时，开始着手将 Swift 引入 Tensorflow 的项目。尽管这个项目在 2021 年被[关闭](https://www.infoworld.com/article/3608151/swift-for-tensorflow-project-shuts-down.html)，我仍然抱有希望。[数字](https://github.com/apple/swift-numerics)包仍在开发中，语言的自动区分也有了进展。能够在 Swift 中完成有意义的数据科学工作也会给程序员带来额外的好处。CreateML 是苹果公司帮助开发者创建 ML 模型的工具，它只提供 cookie cutter 解决方案。在目前的形式下，如果苹果没有你需要的模型，你必须转向 python，自己训练模型，然后在完成后将其转换为苹果的模型格式。在 Swift 中运行整个管道将是一个真正的优势。

## 3.一个充满活力的开发人员社区，为使用其他语言的开发人员采用 Swift 提供教育、故障排除和启发。

讨论的最后一个领域是开发社区，这让我想起了一个老问题，先有鸡还是先有蛋。你知道，哪个先出现？在这种情况下，问题是这样的。如果开发者没有首先采用这种语言所需要的基础设施，我们怎么能在非苹果平台上拥有强大的开发者社区呢？如果开发基础设施的开发人员无法从社区中获得答案，他们如何获得帮助呢？也许这是一个愚蠢的类比，但我认为我们需要探险家和开拓者。人们愿意自己走出去，努力克服 Stack Overlflow 上尚未解决的挑战，并为改善社区贡献他们的时间和代码。

# 结论

在这篇文章的副标题中，我提出了一个问题。" Swift 会成为统治它们的语言吗？"抱歉，那确实是一点点。我认为 Swift 在通用方面确实有潜力，可以做各种各样的语言，但我们还没有到那一步。我们需要一些我在本文中提到的关键构件。一个用于构建 GUI 的框架，更好的脚本支持，一个在非 Apple 平台上等效的 playgrounds，以及额外的低级跨平台工具，这些工具可以作为更高级别的抽象的脚手架。我希望有一天我们能到达那里。我喜欢整理我的项目，让其中一些 100%快捷。

如果你喜欢读这个故事，不要忘记点击那个按钮，当我写新的东西时，跟着我接收更新。