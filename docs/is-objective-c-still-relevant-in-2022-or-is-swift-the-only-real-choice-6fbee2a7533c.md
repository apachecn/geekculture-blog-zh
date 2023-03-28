# Objective-C 在 2022 年仍然相关还是 Swift 是唯一真正的选择？

> 原文：<https://medium.com/geekculture/is-objective-c-still-relevant-in-2022-or-is-swift-the-only-real-choice-6fbee2a7533c?source=collection_archive---------7----------------------->

![](img/d5a652ed6387ddcf583468bce7cc147d.png)

很容易说，无论您想用 Objective-C 实现什么样的最终目标，您都可以用 Swift 来实现。使用 Swift 很有可能会更快得到结果。当然，无论选择的语言是 Swift 还是 Objective-C，开发人员都可能交付应用程序或库。然而，在经过一番思考后，我们无法得出一个边缘案例，让我们倾向于使用 Objective-C 进行 iOS 移动应用程序开发。

*免责声明:我们假设你是一家不仅仅雇佣掌握 Objective-C 知识的开发人员的公司。*

我们知道我们最初的想法对 Objective-C 相当苛刻。我们想指出的是，我们的开发人员大多喜欢用 Objective-C 编程，并且仍然喜欢它交付产品的能力。此外，Objective-C 是一件艺术品，创建者打包了 genius 解决方案并不断改进它，所以我们开发人员能够以我们的优势使用它。

有很多迹象表明，苹果和其他开发者仍在使用大量遗留的 Objective-C 代码。查看 StackOverflow 2021 调查的一些统计数据，我们看到 5.1%的受访者使用 Swift，而 2.8%的受访者使用 Objective-C，比例约为 2:1。不要误解我们，如果你足够努力，你可能仍然会发现对 Objective-C 开发的需求(在这里维护可能是一个更合适的词)。

**让我们来看一些数字。**

# 开发人员数量

当查看精通 Swift 或 Objective-C(或两者兼有)的开发人员的数量时，我们设法从 JetBrains (2021) 中找到了一些统计数据，显示在参与调查的所有开发人员中，76%的开发人员精通 Swift，13%的开发人员精通 Swift 和 Objective-C，而 11%的开发人员仅精通 Objective-C。这些统计数据是不言自明的。

# StackOverflow 票的数量

[在面向所有开发人员的知识库 StackOverflow 上，](https://stackoverflow.com/tags) Swift 在总提问量(316.604 对 292.479)和每周提问量(481 对 39)上都超过了 Objective-C。

# 一种语言的流行

编程语言 PYPL 流行指数是通过分析人们在谷歌上搜索语言教程的频率而得出的。语言教程搜索得越多，假设该语言越流行。Swift 和 Objective-C 在谷歌搜索中的份额相当(Swift 为 2.09 %，Objective-C 为 2.03%)。

在这一点上，我们想指出的是，我们不应该盲目地看数字，而是应该试着理解这些数字是从哪里来的。PYPL 流行指数的创建者说，“一门语言教程被搜索得越多，这种语言就越受欢迎。这是一个领先指标。

原始数据来自谷歌趋势。这个指数是通过分析语言教程在谷歌上被搜索的频率而创建的。但是用“iOS 教程”代替了“Objective-C 教程”。如今，我们可以说由于一些历史原因。但这里的关键要点是，任何搜索“iOS 教程”的人都可能会找到基于 Swift 的教程。

显而易见，这样的数字可能非常具有误导性，给人一种 Swift 和 Objective-C 在[本地 iOS 应用开发](https://www.itmagination.com/services/custom-software-development/mobile-application-development)中同样受欢迎的错误印象。

# 从其他语言导入框架— Swift 为您带来了什么

最近还有一项任务仍然需要 Objective-C 与 Swift 一起用于开发本地移动应用程序。直到最近，如果您想在 Swift 项目中使用 Rust 或 C++库，您需要使用带有桥接头的 Objective-C 作为中间语言，作为从 Swift 到您选择的其他语言框架的连接。

‍ [根据 Thomas Karpiniec](https://www.youtube.com/watch?v=nykPgbxCXZA) 的说法，现在有了 Xcode 13 和 Swift 5.6，你就不再需要 Objective-C 了。正确设置库后，您可以在 Swift 项目中导入和链接它们，并仅在 Swift 中使用它们。

*我们有没有提到，Swift 有独家功能？*

这种类型的比较可能是最有偏见的，或者仅仅是基于观点，但是我们想到的唯一一种完全基于 Objective-C 的技术可能是，如果您想使用 Objective-C 框架的稍微修改的版本。众所周知，如今，在 Swift 项目中导入 Objective-C 框架很容易。但是，您可能需要 Objective-C 框架以这种方式表现出稍微不同的行为，这样，只需修改框架，然后将其导入 Swift，而不是在 Swift 中进行变通以实现相同的目标，这是有意义的。

这是唯一一个可以算作 Objective-C 独家的东西。

另一方面，有许多语言特性您只能通过使用 Swift 来利用。让我们列举几个:SPM、SwiftUI、WidgetKit、Combine、async/await、属性包装器、带有关联值的枚举……用今天的术语来说，这些代表了现代 iOS 开发的最新水平。大多数开发人员都参与了苹果产品(macOS、iPadOS、tvOS、watchOS)的开发。

# SPM—Swift 软件包管理器

根据[swift.org](http://swift.org/)的说法，“Swift 包管理器是一种管理 Swift 代码分发的工具。它与 Swift 构建系统相集成，可自动完成下载、编译和链接依赖关系的过程。”

SPM 包含在 Swift 3.0 及以上版本中。尽管它发布时有一些缺点(例如处理二进制框架)，但现在已经有了很大的改进。现在，我们可以说这是 Swift 项目依赖性管理的标准。事实上，维护开源存储库的开发人员在很大程度上增加了使用 SPM 导入其框架的能力。

# 迦太基和椰子

至于其他依赖管理工具，即 Carthage 和 CocoaPods，我们可以简短地说，它们都允许基于 iOS 和 Objective-C 的项目有相同的工作流。所以这些工具对于编程语言来说没有什么区别。

# 结论

最好将这一章放在文章的顶部，并声明“TLDR:不用动脑筋，Swift 是必由之路”。如果你会看到一些奇特的统计数据使得 Objective-C 得分出奇的高(受欢迎程度，在新项目中的使用，等等。)要怀疑——Objective-C 还在我们中间只是因为遗留原因，只是。[我们 ITMAGINATION 长期以来一直支持 Swift 进行原生 iOS 移动应用程序开发，并准备在您的下一次冒险中与您的团队合作。](https://www.itmagination.com/contact)

‍

【https://www.itmagination.com】最初发表于[](https://www.itmagination.com/blog/is-objective-c-still-relevant-in-2022-or-is-swift-the-only-real-choice)**。**