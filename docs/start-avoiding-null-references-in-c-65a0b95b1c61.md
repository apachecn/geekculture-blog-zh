# 我不再在 C#中使用空引用。以下是方法。

> 原文：<https://medium.com/geekculture/start-avoiding-null-references-in-c-65a0b95b1c61?source=collection_archive---------2----------------------->

*这是一个开始远离防御性编程的简单指南。或者换句话说，使用空引用作为任何代码块的合法输出。*

![](img/9f50968d9dacfc306d6bb5b749356710.png)

Photo by [Joshua Aragon](https://unsplash.com/@goshua13?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这一切都始于理解最初为什么引入空引用，以及[东尼·霍尔爵士如何解释这是一个十亿美元的错误](https://devblogs.microsoft.com/dotnet/nullable-reference-types-in-csharp/?WT.mc_id=DOP-MVP-5003880#the-billion-dollar-mistake)。我不想详述历史，建议你看下面的视频:

# 为什么空引用不好？

基于不同的语言和它们的限制，有很多点可以决定你是否真的想使用空引用。但是空引用在生活中最常见的问题是防御性编程。

> D 防御性编程是一种防御性设计的形式，旨在确保一个软件在**不可预见的情况下**继续运行。

作为一名程序员，你会面临以下一些挑战:

1.  通向假设测试场景的逻辑分支。
2.  为 [NullReferenceException](https://docs.microsoft.com/en-us/dotnet/api/system.nullreferenceexception) 添加 catch 块。
3.  增加了类/代码块的使用者处理空值的责任。

# 那么有什么选择呢？

## 选项 1:在编译时强制空检查

作为 C# 8.0 的一部分，微软确实引入了[可空和不可空引用类型](https://docs.microsoft.com/en-gb/dotnet/csharp/nullable-references#nullable-contexts)来强制所有或任何引用类型对象遵循空检查的项目范围规则。

您可以通过添加一个名为`<Nullable>enable</Nullable>`的属性在项目级别上简单地启用它，并强制开发人员在编译时自己简单地找到任何可能的空值。修改后的 csproj 文件如下所示:

这种方法仍然会让您考虑防御性编程。

## 选项 2(推荐):使用[选项类型](https://en.wikipedia.org/wiki/Option_type)来完全避免空类型

第二种选择更加可控和明智，可以完全避免应用程序中的`NullReferenceException`。对于结果对象的状态，我们将只有两种可能性，即或者是*或者是 ***无*** 。*

*我们将使用 [LanguageExt。Core](https://github.com/louthy/language-ext) 在这个例子中获取实现选项类型的库。*

*让我们以 ASP.NET 核心 Web API 为例，它通过标识符返回客户详细信息。它基本上是一个接受客户 id 的 GET 端点。场景是:*

*   *客户详细信息存储在内存缓存中。*
*   *将通过 Id 搜索客户详细信息，并返回给 API 调用者。*
*   *如果客户存在于缓存中，则返回 200 (OK)。否则，404(未找到)。*

*LanguageExt。Core 提供了一个名为 ***Option < >*** 的类型，允许你简单地将函数的返回类型定义为选项类型。在下面的例子中，我添加了一个名为 CacheProvider 的类来管理缓存。参见下面的要点(第 12 行),如果键匹配，则返回缓存对象，如果键不匹配，则返回`**Option<TResult>.None**`。*

*这使得 CacheProvider 类的使用者在验证响应时更加容易。*

*处理空引用时，典型的检查是这样的:*

*请看下面(第 17 行)我们如何使用`Match<IActionResult>()`向 API 消费者提供适当的响应，而不是添加一个空检查，然后基于此进行响应:*

## *你的测试会是什么样的呢？*

*单元测试 CacheProvider 变得同样简单，因为我们在这里并不关注为空引用检查编写测试。看看第 27 行和第 41 行的断言。*

*感谢您花时间撰写本文。希望有帮助。*