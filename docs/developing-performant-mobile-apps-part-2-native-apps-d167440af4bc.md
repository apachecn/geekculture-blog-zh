# 开发高性能移动应用第 2 部分:本地应用

> 原文：<https://medium.com/geekculture/developing-performant-mobile-apps-part-2-native-apps-d167440af4bc?source=collection_archive---------31----------------------->

![](img/e2e3da605d4454304b6e889bf13aba8a.png)

现代移动应用程序开发始于第一代 iPhone。尽管[渐进式网络应用](https://www.itmagination.com/blog/cross-platform-mobile-apps-pwa)是第一个为该设备提出的，但它们有一些紧迫的问题。它们运行缓慢，而且很难创建看起来像本地人的界面。

苹果在他们宣布后迅速发布了补救措施:iOS 原生 SDK。快进到 2021 年，创建原生应用是默认的。他们只是比他们的非本土竞争对手更快，性能更好。

# 速度=更好的用户体验

速度是所有 app 的终极特性。[能够稳定保持稳定的每秒 60 帧阈值而没有任何渲染延迟的移动应用](https://www.itmagination.com/services/custom-software-development/mobile-application-development)感觉最好。[必须确保任何中断不超过 400 毫秒](https://lawsofux.com/doherty-threshold/)。

自然，实现目标的最简单的方法是使用将被手机的操作系统直接评估的编程语言。在其他情况下，你的指令必须首先被翻译才能被理解。这是一个相当昂贵的过程，而且不是最快的。

# 新一代语言

Objective-C 和 Java 并不是世界上最受欢迎的语言。[在 2020 年](https://insights.stackoverflow.com/survey/2020#technology-most-loved-dreaded-and-wanted-languages-loved)的最新堆栈溢出调查结果中，44%的开发人员宣称他们“热爱”Java，Objective-C 的收益**只有区区 23%** 。如果被调查者“正在用这种语言发展[……]并且已经表示有兴趣继续用这种语言发展”，他们就被认为是热爱这种语言

相比之下，科特林的回报率为 62.9%，而斯威夫特为 59.5%。JetBrains 的语言修复了许多困扰 Java 的问题，例如新语言更好地处理了“null”值，并且可以避免编写大量“样板”代码。

## 迅速发生的

与它的前身 Objective-C 相比，Swift 也可以被视为一大进步。首先，该语言是静态类型的，可以提前捕捉错误。此外，开发这种语言的公司表示，新语言比以前的语言快 2.6 倍。

此外，苹果还在不断改进这种语言。今年，在 WWDC 期间，来自库比蒂诺的传奇公司宣布了该语言和更广泛的 iOS 开发生态系统的一些令人兴奋的功能。这种语言现在更好地利用了记忆。它会比以前更快地自动回收内存。

现在你可以编写异步/等待代码，使得非阻塞代码更容易编写。第三，以及[“结构化并发”](https://github.com/apple/swift-evolution/blob/main/proposals/0304-structured-concurrency.md)(并发通常是指同时执行代码)，该领域的改进肯定会让所有 Swift 开发人员兴奋不已。新增加的功能允许更安全、无问题地实现更快的[应用](https://www.itmagination.com/services/custom-software-development/mobile-application-development)。

## 科特林

我们过去已经谈论过 kot Lin——IT magination 的 Android 开发人员 Jan Radzikowski 在我们的每周系列 [360 IT Check](https://www.itmagination.com/newsletters/360-it-check) 中分享了他过去对 JetBrains 的编程语言[的积极看法:](https://www.itmagination.com/blog/360deg-it-check-7-kotlin-10-years-cybersecurity-talent-stack-overflow-survey-pixel-6-tensor-ai-chip-windows-365)

> *我会说 Kotlin 比 Java 更适合我，但我是从 Android 开发者的角度来说的。我想到的第一个好处是易读性。科特林非常简洁，可以用这样一种方式，你可以读它几乎是一部小说。
> kot Lin 相对于 Java 的‍Another 优势是空引用由[Kotlin 的类型]系统控制(NullReferenceException anyone？).你根本不用给它们做注解。*
> 
> 从细节上看，没有原始类型，在我看来，这使得语言更加一致。还有什么？
> 
> *- Lambdas 和内联函数
> -扩展函数！—它们产生了巨大的差异，并提高了可读性和编码速度(当然是在正确使用的情况下)
> —主构造函数的概念
> —数据类
> —对象以及 Kotlin 如何处理单态对象*
> 
> *最后，协程是一大优势。它们是取代 RxJava 库的好方法，并且能够编写易读且线程安全的异步代码。*

本节的要点很简单——由于新语言的出现，原生开发体验在不断改进。

# 设备的完整功能集

并不是所有的 API 都适用于编写 [PWAs](https://www.itmagination.com/blog/cross-platform-mobile-apps-pwa) 或混合应用的开发者。有时，人们不得不找到解决问题的替代方案，或者编写本机代码。总而言之，到最后，如果你被迫进行原生开发，那你还不如全押，对吧？尤其是我们不是在讨论高级功能的使用。例如，要在 React Native 中实现日历事件创建，您必须从您的 JavaScript 调用 Swift 和 Kotlin 代码——没有其他方法。

# 本机界面元素和实践的应用

正如我们之前提到的，你的用户将最熟悉其他应用程序的[设计，而不是你的。人们可以很容易地利用这一事实，通过设计你的应用程序给用户相同的反馈，相似的工作，并利用相同的视觉模式。人们在哪里度过的时间最多？当然是系统内部的应用程序。](https://www.itmagination.com/services/custom-software-development/product-design-ux-ui)

如果您正在编写代码，使用本机模式是默认选项。默认情况下，你的 app 会感觉熟悉很多；就位。

当你在编写一个可安装的 web 应用程序，或者一个跨平台的应用程序时，这一点并不明显。Web 应用程序不能使用原生 UI 元素，而一些跨平台框架，如 Flutter，放弃了原生元素的选项，而是使用 Flutter 的 Skia 引擎来呈现自定义组件。当然还有 React Native，用的是原生元素；但是，它并没有实现所有的功能。

# 双倍代码，双倍更新

如果你打算采用真正的本地方法，这意味着你将不得不两次编写相同的应用程序。一次用 Swift，一次用 Kotlin。当然，不同语言的算法实现可能或多或少是相同的，因此您不会在开发上花费两倍的时间。您可以尝试使用应用程序从服务器接收的数据来确定屏幕上的内容。

[几年前，Airbnb 描述了他们如何试验服务器驱动的应用](/airbnb-engineering/whats-next-for-mobile-at-airbnb-5e71618576ab)，作为“取消设置”React Native 并转向原生开发系列的一部分。服务器驱动的应用程序是基于手机从后端接收的数据呈现屏幕的应用程序。因此，所有平台上的代码看起来或多或少都是一样的，同时减少了大量开发时间。这大概就是好消息了。特定于平台的错误仍然会发生，在每个操作系统上产生不同的问题，雇佣特定于平台的开发人员的需要仍然会推高成本。

# 结论

本地应用将是最快的。他们也会让你的用户感到最熟悉。它不会没有一定的权衡。开发时间肯定会更长。它们的开发成本也要高得多，因为你需要更多的人和资源来两次编写同一个应用程序。

原生应用、渐进式网络应用或混合应用— [我们在这里帮助您开发您的应用](https://www.itmagination.com/services/custom-software-development)。

*最初发表于*[T5【https://www.itmagination.com】](https://www.itmagination.com/blog/native-mobile-apps)*。*