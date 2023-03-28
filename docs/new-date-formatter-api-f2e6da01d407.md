# Swift 中新的日期格式化程序 API

> 原文：<https://medium.com/geekculture/new-date-formatter-api-f2e6da01d407?source=collection_archive---------10----------------------->

## 通过大量有用的示例掌握新的格式化程序 API

![](img/25f355dca44e800f6aef3697aae483e5.png)

Photo by [AltumCode](https://unsplash.com/@altumcode?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/ios-coding?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

从 Swift 5.5 和 iOS 15 开始，我们有了一个新的格式化程序 API，允许我们以更具声明性和直观的方式显示字符串日期。在我们深入研究之前，让我们回顾一下实际的格式化程序 API 是如何与下面的例子一起工作的。

这已经足够了。我们使用[静态工厂模式](/swlh/static-factory-pattern-f1d4897ebc3d)来创建一个可重用的数据格式化程序，然后，我们创建一个实用函数，在给定特定格式化程序的情况下，将日期解析为字符串。

这种方法的问题是，如果我们想以不同的格式显示日期，我们需要创建另一个 DateFormatter。除此之外，我们没有通过“硬编码”格式来考虑用户的语言设置。例如，在西班牙语中，日期格式遵循 **dd/MM/yy** 模式，而不是 **MM/dd/yy** 。

新的格式化程序 API 解决了这个问题，让我们有可能描述我们想要显示日期的方式，而不是配置日期格式化程序。我们现在有四种格式功能可供使用:

让我们从最基本的例子开始，使用选项一。这将把我们的日期转换成默认的字符串格式(日期+时间)。

我们无法进行任何转换，但有些情况下，这种格式对我们来说已经足够了。如果我们想定制日期格式，我们需要使用第二个选项。

开始的时候有点棘手，但是一旦你明白如何实际使用它，就会变得容易。

我们需要提供一个 FormatStyle 作为参数来使用该函数。

这种 FormatStyle 是一种具有两种关联类型的协议:输入和输出。在这种情况下，输入需要是日期类型。幸运的是，Swift 已经为我们提供了一个符合我们需要的 FormatStyle 协议的结构: **Date。格式样式**。这个 FormatStyle 有一个静态变量`dateTime: Date.FromatStyle`，我们可以将它直接用作函数的参数。

我们现在有了一个 FormatStyle 实例，可以使用 **Date 对其进行定制。FormatStyle** 的实例方法(可以查看官方文档中的列表)。让我们看几个例子。

这里我们取日期时间变量(它是一个日期。FromatStyle 实例)并调用月、日、年函数。这三个函数都返回一个日期。FromatStyle 类型，这是函数的参数所期望的类型。我们只是在操纵格式。使用这些实例方法，我们可以实现很多组合。

请注意，我们调用函数的顺序不会影响最终的输出。Swift 为我们做了繁重的工作，并根据用户的喜好决定正确的格式。

现在假设我们需要向后端发送一个特定格式的字符串日期？(比如“2021–07–18”)。对于这个场景，我们需要使用第三个选项。比如约会。FormatStyle，有一个静态变量供我们使用，iso8601。这个格式函数很像我们之前看到的那个，但是有一些额外的配置。

使用{\\ f39 format style ,}我们可以指定想要使用的日期分隔符。

现在，如果我们只对显示日期字符串感兴趣，但对格式没有具体要求，我们可以使用最后一个选项(第四个)和一些预定义的格式。两个日期。格式样式。日期样式和日期。FormatStyle.TimeStyle 为我们提供了几个现成的静态常量。

最后缺少的部分是相反的，我们如何从一个特定格式的字符串创建一个日期类型？为此，我们必须使用新的日期初始值设定项:

我们必须向函数传递一个字符串(它将是具有自定义格式的字符串类型中的日期)和一个解析该字符串的策略。这个策略参数必须是一个 **ParseStrategy** 类型。和**格式样式**一样，**解析策略**也是一个协议，也有两个相关的类型(输入和输出)。输入必须是字符串，输出必须是日期。

对我们来说，好消息是，像**format style。Date** ，我们已经有了一个符合 **ParseStrategy** 协议的内置结构: **Date.ParseStrategy.** 我们只需要创建一个新的实例，在 Date 的 init 函数中使用它作为参数。

假设我们将从后端接收一个日期字符串，格式如下:dd-MM-yyyy(例如 31–01–2021)。让我们首先创建我们的 ParseStrategy 实例，然后使用 parse 创建一个新的 Date 实例。

因为格式是日期。FromatString 类型，我们可以使用插值初始值设定项结合日期。创建我们的日期格式的符号。

请注意，在我写这篇文章的时候，所有这些功能都处于测试阶段，在正式版本中可能会有变化。

我已经创建了一个备忘单，其中包含了您可以使用新 API 做出的大多数变化。你可以在这里查看[。](https://github.com/blorenzo10/swift-utilities/blob/master/Common%20/DateFormatter.swift)

一如既往，如果你有任何问题，在 [Twitter](https://twitter.com/b_lorenzo10) 上留言或 DM，我很乐意帮忙=)。