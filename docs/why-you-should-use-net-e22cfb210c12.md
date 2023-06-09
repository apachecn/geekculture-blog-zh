# 为什么您应该使用。网

> 原文：<https://medium.com/geekculture/why-you-should-use-net-e22cfb210c12?source=collection_archive---------19----------------------->

![](img/5108167c7244647405b49c6023ddc3ba.png)

# 介绍

本文旨在分析微软的框架。NET，以前称为。网芯。无论如何，我都不想给你洗脑。而是为您提供了这个框架所能提供的概要视图。我已经做了 6 年的. NET 开发人员，第一年是在各种大学项目中，其余时间是在我的职业生涯中。虽然这种经历并不能使我成为框架方面的专家，但是我希望向您保证，至少这篇文章是来自一个对框架有经验的人。

# 框架 vs 库

在我们开始之前，我想澄清一个成熟的框架和一个库之间的区别。在过去的几年里，我看到一些被认为是框架的技术，当你仔细观察它们的时候，它们本身几乎不起作用，需要其他插件。想到的一个例子是 React。在一些开发人员的大脑中有一点误解，他们认为 React 是一个框架，但它不是。它是一个专门渲染 UI 的库。框架为你提供了一个主干，一个骨架，你的应用程序将建立在它上面。理论上，框架已经为您提供了开发应用程序所需的所有工具。

**注意:**对于正在阅读本文的任何热情的 React 开发人员。请不要因为我的比较而生气，我只是尽可能地简化，为读者提供一个框架应该包含的清晰图像。

# 你能用它做什么？网？

在我们进入框架的技术评估之前，让我告诉你你用它做什么。基本上什么都有！这是一个充满特性的框架，可伸缩，安全，跨平台，性能非常好，而且最重要的是，它是免费的！

顺便说一下。NET 被认为和设计成我们可以用它创建所有类型的应用程序，比如 web API(ASP。NET)、网络应用、桌面(WPF)、移动(Xamarim)、物联网，甚至视频游戏。你可能用 C#和。NET，但与其他语言如 Java、Scala 或 python 相比，它有所欠缺。

你可能不知道这一点，但我们的“主和救世主”堆栈溢出是用 C#使用 ASP.NET MVC 编写的。

你可能已经发现了。NET 你不会被限制在 Windows 环境中，任何用这个框架开发的应用程序都可以在任何环境下运行，比如 Windows、Linux、iOS、Android 等等。

# 为什么用。网？

## 易用性

。NET 是一个开源平台，拥有种类最多的库和工具，可以帮助您集成企业中随处使用的所有类型的数据库和云服务(Google Cloud、AWS、Azure)。

## 代码维护

与。NET 你将会以这样一种方式构造你的代码，使得代码可以很容易地被分离到类库甚至定制包(NuGet)中，如果你愿意的话。这种类分离允许您的模块和代码片段在其他项目中重用。

## 可攀登的

。NET 的设计方式使得您的应用程序可以非常轻松地进行垂直和水平伸缩。您不需要担心实现您的工具来处理项目中的新挑战。

下面是一些现有工具的很好的例子，您肯定需要这些工具来垂直扩展您的项目:

*   **任务并行库**为您提供线程、任务和异步编程，以帮助您在项目中添加并发性。
*   **Parallel Linq** 为您提供了 Linq 标准的实现，作为对现有数据集合(比如列表)的扩展方法。
*   用于并行编程的专用**数据结构**。。NET 提供了几种在并行编程中有用的类型，包括一组并发集合类、轻量级同步原语和用于惰性初始化的类型。您可以将这些类型用于任何多线程应用程序代码，包括任务并行库和 PLINQ。

**注意:**如果你使用微软的企业级集成开发环境 Visual Studio，你还可以使用非常强大的诊断工具进行监控和开发。

## 和睦相处

过去的一个问题。NET 核心，是兼容性。如果你在。NET，你需要在 Windows 机器上运行它，但是现在有了新版本的。NET 不再是这种情况，您的应用程序可以很容易地在不同类型的设备上运行

最常见的是，这个框架使用 C#作为它的语言，但它的构建方式允许它兼容多种语言，如 VB.NET、F#和 c++。C#是一种**通用**编程语言，它为众所周知的‘C-esque’编程语言提供了一种更简单、更安全、更现代的方法。

## **标准化**

由于框架附带了各种各样的库，在大多数情况下，您不需要任何外部库和标准，比如 MVC。通过这种标准化的方法，您可以在团队之间更加自由地移动，并且能够非常快速地赶上进度，使您和您的知识可移植。

## 云友好

这里没有什么令人震惊的，因为微软本身也是一个云提供商，所以这个框架是在考虑云计算的情况下构建的是完全有道理的。

## 可靠性

的。NET 框架并不是一个新的框架，在长达 15 年的时间里，它一直在发展和改进它的缺点。框架本身过去是封闭源代码的，直到最近才有所改变。

有一个独立的基金会，名叫。NET Foundation，由 3.7k 家公司和 10 万多名开发者组成，他们为. NET 的改进做出了贡献。除此之外，还有由 Amazon、Google、Red Hat 等公司组成的指导小组..它积极地引导着世界的未来。NET 平台。

此外，现在随着框架的开源，您可以在更短的时间内获得补丁和修复，而在过去，您必须等待微软的重大更新，这可能需要数年时间。

所有这些加起来使得这个框架非常可靠，是对您未来项目的良好投资。

# 为什么你不应该使用。网

虽然我可能不同意某些观点，但我认为了解这项技术可能存在的缺点是很重要的，尤其是在我们进行技术评估的时候。因此，如果我不同意，我将向你提出一些观点和我的反驳。

我听到有人说 **C#相对较新，开发社区与 C#脚本**不合拍。在我看来，这并不是一个不学不用的站得住脚的理由。NET 和 c#。虽然 C#可能比其他语言更新，但它已经问世近 21 年了，而且该语言本身的设计与其他 C 风格的编程语言相似，例如，一个一生都在使用 Java 的开发人员可以相对容易地过渡到 C#。

**云服务提供商可能没有合适的文档来整合你的项目。**我同意这一点，当然，只有 Azure 完全符合您的需求，但这是意料之中的，因为它是微软的产品。无论如何，缺少文档并不意味着不兼容。

**C#不利于大数据。**我之前在文章中提到过这一点。虽然有选择的余地。我不得不同意，如果你的项目需要数据分析，你最好使用另一种技术和语言。

**有些图书馆不提供某个产品的完整功能(如 Kafka 流)。我还没有遇到过这个问题，但很自然，我同意这一点。。NET 并不是一项完美的技术，而且还在不断地更新和改进，所以如果某个库还没有完全开发出来，我们可以预计它在将来会开发出来。**

# 我能找到一份. NET 开发人员的工作吗？

当然，这是一个非常流行的框架，全世界的公司都在他们的项目中使用它。有些人甚至可能专门从事 ASP.NET/Azure 类型的项目，并且只从事这方面的工作。截至 2021 年，约有 4%的工作机会面向。NET 和 6%的 c#。

我建议你不要简单地决定成为一名. NET 开发者。我相信任何认为自己是 X 框架/库开发者的人都是错的，你应该总是努力学习更多不同的东西。例如，作为一个后端开发人员，如果你只是用。NET 和 c#并拒绝学习其他任何东西，你只是在削弱自己。

# 结论

。NET 对于任何公司的项目来说都是一个很好的框架，因为它提供了一个可伸缩的、可靠的、安全的和稳定的结构来构建伟大的新项目和想法，但是我们应该永远记住，仅仅因为我们知道如何使用锤子，并不意味着其他一切都是钉子。所以在收养之前。请确保该工具符合您的需求。