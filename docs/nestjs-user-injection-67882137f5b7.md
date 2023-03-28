# NestJS 用户注射

> 原文：<https://medium.com/geekculture/nestjs-user-injection-67882137f5b7?source=collection_archive---------0----------------------->

NestJS 是最受欢迎的 Node.js 框架。它利用 TypeScript 的全部功能使开发过程变得快速而简单。在本文中，我将分享非常酷和有用的技巧“用户注入”，它可以极大地简化您的 NestJS 项目的代码。

![](img/9503cd9bfb80651d05b30e92987abe75.png)

# 我为什么需要这个？

这是任何开发者在开始新的东西之前应该问的第一个问题。让我们想象一下，你有一个 API 服务器，你需要限制对一些数据的访问，这是一个很常见的场景。作为一个例子，让我们为一个在线商店创建一个简单的 API，在这里我们有订单和客户。客户只能访问自己的订单，不能访问其他客户的订单。`OrdersController`会是什么样子:

这个控制器用`AuthGuard`包裹，`@Customer`用于检索当前用户。这里是`OrdersService`的代码:

你可以看到`customer`被传递给了`OrdersService`的每个方法。这只是一项服务。如果我们有数百个服务，每个服务至少有 10 个方法，那会怎么样？用客户的数据传递一个额外的参数真的很烦人。

# 解决办法

幸运的是，NestJS 提供了非常灵活的依赖注入机制，NestJS 从第 6 版开始支持请求注入范围[https://docs . NestJS . com/fundamentals/injection-scopes # request-provider](https://docs.nestjs.com/fundamentals/injection-scopes#request-provider)。这意味着我们可以将当前请求注入到我们的服务中。如果我们可以注入请求，我们就可以注入存储在请求中的用户数据。让我们写一个客户提供者:

现在尝试用它将客户注入`OrdersService`:

现在 NestJS 为每个请求创建了一个新的`OrdersService`实例，让我们发出一个请求，看看注入了什么:

`Current customer: undefined`

![](img/1589c8249f3f6bc393084ccec7dcc5da.png)

应该可以，但实际上`undefined`是注入到`OrdersService`的。这种意外行为的原因是 NestJS 的依赖注入机制。一收到新请求，它就创建所有的提供者，此时`req`对象不包含发出请求的客户的信息。验证授权令牌/cookie 和从数据库接收用户数据需要一些时间。

# 解决方案。尝试#2

我花了一些时间想出一个优雅的解决方案。如果在注射的时候没有可用的东西，我们如何注射呢？但是解决方案很明显，如果用户的数据还不可用，让我们把它包装成一个 getter:

现在我们可以在`OrdersService`中使用它:

而在`OrdersController`中:

如您所见，代码变得更加清晰。不再需要将`customer`传递给每个方法，它在`OrdersService`中可用。

你可能会问，当某个方法被调用时，我们如何确定`customer`已经被初始化了呢？这是一个公平的问题。如果`@UseGuards(AuthGuard)`被添加到控制器类，NestJS 首先执行 guard 的`canActivate`方法。该方法检查当前授权数据，并将其添加到请求对象中。换句话说，如果使用了授权保护，我们可以确保在运行`OrdersService`中的某个方法之前，客户数据已经被初始化。

# 下一步是什么？

如果我们可以注入一个用户，我们可以用同样的方式注入它的权限。用 CASL 很容易实现[https://docs . nestjs . com/security/authorization # integrating-casl](https://docs.nestjs.com/security/authorization#integrating-casl)。我们甚至可以更进一步，将用户注入到存储库中，并根据用户在数据库级别的权限过滤数据。这种技术的潜在用例只受您的想象力的限制。

# 骗局

任何方法都有利有弊，“用户注入”也不例外。

*   性能。这种方法基于请求范围注入，这意味着所有必需服务的新实例将在每个传入请求上创建。使用默认注入范围，当应用程序启动时，所有服务都被初始化。但是，实例创建仍然是一个非常廉价的操作。我想补充一些真实的性能比较，但用例是如此多样。性能测试结果只会显示我的项目的差异，而对于您的项目，它们可能会显示完全不同的画面。
*   测试。为请求范围的服务编写测试比具有默认注入范围的服务要难一些。你可以从官方文档[https://docs . nestjs . com/fundamentals/testing # testing-request-scoped-instances](https://docs.nestjs.com/fundamentals/testing#testing-request-scoped-instances)中找到这个主题。但另一方面，我们可以在一个地方模仿用户的数据，而不是模仿每个服务的方法。

# 结论

NestJS 是一个灵活的功能性框架。其依赖注入机制的潜力几乎是无限的。用户注入是许多技巧中的一个，可以和 NestJS 一起使用。我并不认为这是组织代码的唯一正确方式。但是我希望这种方法对您有用，并帮助您重新思考代码结构。

*下次见！Servus！*