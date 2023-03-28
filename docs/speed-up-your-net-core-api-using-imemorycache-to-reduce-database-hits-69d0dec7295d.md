# 加快你的。NET 核心 API 使用 IMemoryCache 来减少数据库命中

> 原文：<https://medium.com/geekculture/speed-up-your-net-core-api-using-imemorycache-to-reduce-database-hits-69d0dec7295d?source=collection_archive---------5----------------------->

![](img/434cb7c2524fd09485e947f857242c5a.png)

Photo by [Marc-Olivier Jodoin](https://unsplash.com/@marcojodoin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在我最近参与的一个项目中，我们有一个从数据库返回大型数据集的查询，但是数据基本上是静态的，并且每年更新一次。每次用户访问网站时，他们都必须等待几秒钟，一些下拉列表才会填充。这听起来可能不多，但是如果用户不必等待，为什么还要让他们等待呢？此外，我不喜欢每次都必须访问数据库，这让我很不舒服。

我以前听说过使用缓存，所以我开始深入研究。NET 核心解决方案。粗略研究之后，我惊讶地发现使用缓存是如此简单和容易。每当用户打开我的网站时，不再需要几秒钟就返回数据，现在查询从缓存中提取，响应是即时的。

让我向您展示一下我是怎么做的，以及在中建立自己的缓存是多么容易。网络核心

# 将 IMemoryCache 添加到 DI 注册中

我们很多人使用的原因是。NET Core 是因为它是一个框架，它有一些令人惊奇的内置特性。开箱即用的服务注册数量非常惊人，在内存中添加缓存也不例外。

在项目的 **Startup.cs 文件**中，在 **ConfigureServices** 方法内部。您需要添加的只是这行代码和。NET 将允许您在应用程序中的任何地方注入 IMemoryCache。

```
services.AddMemoryCache();
```

搞定了。这几乎太容易了。接下来，我们将查看一些查询和更新缓存的简单用例

**注意:**你将需要**微软。extensions . dependency injection**包，如果您还没有将它添加到 Nuget 包中的话

# 使用 IMemoryCache

现在我们已经注册了内存缓存，我们可以在应用程序中需要的地方注入这个服务。

在上面的代码中，我创建了一个简单的服务来处理我们的缓存操作。我们在构造函数中注入了两个依赖项，一个用于使用实体框架的 DB 上下文，另一个用于我们在上一节中添加的 IMemoryCache。

在 GetDataAsync 中，我们做了几件事

*   检查我们的缓存中是否存在该密钥
*   如果它不存在，调用数据库并检索数据，设置一个过期时间，然后使用 CacheKey 将条目添加到缓存中
*   如果它已经存在，则返回存储在缓存中的数据

通过运行 _cache。TryGetValue，我们可以很容易地检测到缓存键和值是否已经被添加到应用程序的 IMemoryService 中。如果它返回 false，我们检索数据，否则它将被分配到我们的列表并返回。

为了便于组织，我更喜欢将我所有的缓存键保存在一个单独的静态文件中，这样您就可以跟踪您使用了哪些键，以防止意外的重叠。

**注意:**缓存将只存储字符串，所以您可能想要**序列化和反序列化对象来存储/检索它们。** [Newtonsoft](https://www.newtonsoft.com/json/help/html/SerializingJSON.htm) 是一个很棒的软件包，你可以用它来做这件事。

# 包裹

我希望这篇文章易于理解，并展示了在中设置 IMemoryCache 是多么简单和容易。网芯。如果您有任何问题或意见，请在下面添加，否则，如果您想看到更多类似的内容，请留下掌声并关注我。编码快乐！