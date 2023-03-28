# 如何在 JsonApiDotNetCore 中处理 API 密钥认证

> 原文：<https://medium.com/geekculture/how-to-handle-api-key-authentication-in-jsonapidotnetcore-b21b3ebec467?source=collection_archive---------9----------------------->

## 保持**符合**JSON:API**规范的解决方案。**

![](img/ab879b0662adb9f5c9d1b94b20bdd992.png)

Photo by [George Prentzas](https://unsplash.com/@georgeprentzas?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

非常私有或公共的 API 需要某种形式的认证和授权来保护(某些)资源，防止恶意请求和限制资源使用。

本文将结合**开源** [JsonApiDotNetCore](https://github.com/json-api-dotnet/JsonApiDotNetCore) 框架重点介绍长期 API 键的实现。

*JsonApiDotNetCore 是一个用于构建*[*JSON:API*](https://jsonapi.org)*兼容 REST APIs 的框架。NET 核心和实体框架核心。*

尽管我们将只关注 API keys 方法，但这并不意味着它是每个项目的正确解决方案。API keys 方法在软件行业中被广泛使用，但是就安全性而言，它不是最好的方法。

# 认证和授权

身份验证和授权是相辅相成的，但是为了防止混淆，概述两者之间的主要区别是很重要的。

## 证明

认证的目的是证明你的**身份**。或者换句话说，证明你就是你所说的那个人。

一个常见的例子是使用用户名和密码登录网站(如 Medium)。在这里输入正确的组合将允许您访问您的帐户。

## 批准

授权步骤的目的是证明您的访问权限。证明你被授权提出某个请求。

例如，确定您是否有足够的**权限**来编辑网站上的资源(例如，Medium story)。其中足够的权限允许您对资源执行操作。

# 自定义身份验证处理程序

在我最近的一个项目中，我决定使用长期 API 密匙作为默认的认证方法。与其他身份验证方法(Cookie、JWT 和 OAuth)相比，API keys 方法没有现成的实现。因此，我们必须创建自己的身份验证处理程序。

我们从定义身份验证方案选项模型开始。

[https://gist.github.com/VincentVrijburg/03fd4cf7cf9f5298fabc9924986a0865](https://gist.github.com/VincentVrijburg/03fd4cf7cf9f5298fabc9924986a0865)

我们现在不需要任何自定义选项，但是你可以，例如，将选项从你的`appsettings.json`传递到这个`KeyAuthenticationOptions`(例如，在你的`Startup.cs`中)以便在处理程序中使用它们。

现在，在我们实现实际的认证处理程序之前，我们将首先创建一个`AuthenticationService`来处理认证过程。可以说，这将把数据访问逻辑与我们的身份验证逻辑分离开来。

[https://gist.github.com/VincentVrijburg/02d54982eeda792163c323e3352eb836](https://gist.github.com/VincentVrijburg/02d54982eeda792163c323e3352eb836)

[https://gist.github.com/VincentVrijburg/1b22b7e0c4192c6dc8df50ddee78fb93](https://gist.github.com/VincentVrijburg/1b22b7e0c4192c6dc8df50ddee78fb93)

在这个例子中，`[KeyRepository](https://github.com/VincentVrijburg/JsonApiDotNetCore.Demo.Auth/blob/2d6b210f2fbc007333275ba4c81e9d84bd561030/src/JsonApiDotNetCore.Demo.Auth.Data/Repository/KeyRepository.cs)`是一个模拟数据存储，其中有一些虚构的 API 键值。通常，您会访问存储在数据库或密钥管理服务中的 API 密钥。方法`IsAuthenticatedAsync`的要点是检查所提供的 API 密钥是否有效。

稍后，我们将添加这个`IKeyAuthenticationService`作为一个范围依赖。这将允许我们在请求过程中的任何时候(在认证步骤成功之后)读取设置的属性(例如 UserId)。

至此，我们拥有了组成实际身份验证处理程序的所有必要元素。

注意，我们还没有定义定制响应。基本实现现在会处理这个问题。

既然我们已经构建了所有的身份验证组件，剩下唯一要做的事情就是在身份验证管道中注册它们。

在您注册了创建的身份验证处理程序之后，作为您的默认身份验证方案，默认中间件将处理剩下的工作。就这么简单！

现在，这个实现对于大多数 API 项目来说是一个很好的开始。然而，如果您决定使用 JsonApiDotNetCore 框架构建您的 API，我们必须提交一些额外的更改，以便 1)能够使用我们的自定义身份验证处理程序，2)保持符合 [JSON:API](https://jsonapi.org) 规范。

# 在 JsonApiDotNetCore 中处理身份验证

在(ASP)的早期版本中。NET 核心，您可以创建一个返回自定义身份验证处理程序的中间件。然而，他们将这种方法改变为单一中间件设置。正如我们已经看到的，当前的方法是添加自定义身份验证处理程序作为一个方案，并用默认的`.UseAuthentication()`注册它。

尽管这种新方法简化了身份验证的实现，但它使 JADNC (JsonApiDotNetCore)的集成变得复杂。

由于认证中间件需要在 JADNC 中间件之前注册，所以我们不能依赖 JADNC 内置的错误响应序列化器。因此，我们必须手动实现一个来保持符合 [JSON:API](https://jsonapi.org) 规范。

我们首先实现一个`ErrorResponseWriter`来处理 JADNC 范围之外的异常。

我们可以在我们的定制认证处理程序中直接使用这个`ErrorResponseWriter`,但是我们将在这个响应编写器之上构建另一层。在身份验证和授权管道的不同级别捕获异常的层。

我们通过创建一个通用的异常中间件来实现这个概念。

此时，我们可以在身份验证处理程序中定制响应。

如您所见，我们选择了覆盖两个方法，并相应地抛出异常。

既然我们已经构建了所有的身份验证和错误响应组件，剩下唯一要做的事情就是在应用程序管道中注册它们。

现在，我们已经在 JsonApiDotNetCore 中成功实现了 API 密钥认证，同时遵守了 [JSON:API](https://jsonapi.org) 规范。

看看 GitHub **库**，看看在 [JsonApiDotNetCore](https://github.com/json-api-dotnet/JsonApiDotNetCore) 中基于 API 密钥的认证的完整实现，作为一个演示项目。

[](https://github.com/VincentVrijburg/JsonApiDotNetCore.Demo.Auth) [## VincentVrijburg/JsonApiDotNetCore。演示验证

### JsonApiDotNetCore 框架中基于密钥的认证的演示实现。用 dotnet 恢复 NuGet 包…

github.com](https://github.com/VincentVrijburg/JsonApiDotNetCore.Demo.Auth) 

# 参考

我用来写这篇文章的来源的完整概述。

**JSON:API 规范**
网址:[https://jsonapi.org/](https://jsonapi.org/)

**JsonApiDotNetCore**
资源库:[https://github.com/json-api-dotnet/JsonApiDotNetCore](https://github.com/json-api-dotnet/JsonApiDotNetCore)
网站:[https://jsonapi.net/](https://jsonapi.org/)

**JsonApiDotNetCore。演示授权**储存库:[https://github.com/VincentVrijburg/JsonApiDotNetCore.演示验证](https://github.com/VincentVrijburg/JsonApiDotNetCore.Demo.Auth)