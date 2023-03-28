# 你的 REST APIs 真的是 RESTful 的吗？

> 原文：<https://medium.com/geekculture/are-your-rest-apis-really-restful-a6459406d52?source=collection_archive---------38----------------------->

REST 是分布式超媒体系统的一种架构风格。遵循 REST 架构的 API 通常被称为 RESTful。
写这篇文章背后的动机是我自己长期以来对 REST 的误解。仅仅使用 HTTP 作为传输协议并不足以将 web API 归类为 RESTful(这是我最初用来分类它们的方式:p)。事实上，REST 本身是独立于传输协议的。也就是说，本文假设 HTTP 是底层传输协议。

![](img/e4a9e377a3b388d8c697cb1837e8f31d.png)

Photo by [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/interface?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

本文试图解释在设计一个真正 RESTful 的 API 时应该遵循的约束，但是我应该提到，开发人员应该选择他们认为对他们的用例有益的东西，而不是教条地对待这些约束。比如，如果你同时控制 API 的客户端和服务器端，那么使用一些流行的 RPC 实现(比如 gRPC)来设计 RPC 风格的 API 可能会更加高效和快速。解决了这个问题，让我们开始研究 REST APIs。

罗伊·菲尔丁在 2000 年的著名论文[中首次描述了 REST。REST 旨在通过定义客户端和服务器交互的一些约束，在客户端和服务器之间定义一个统一的连接器接口。它根据资源定义了客户机和服务器之间的交互，客户机请求获取/更新资源(或资源集合)，服务器以 JSON 和 XML 等简单格式返回所请求资源的表示。HTTP 动词可以用来描述需要在资源上执行的操作。以下是 REST 架构的不同原则:](https://www.ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)

# **客户端-服务器**

用户界面问题应该与数据存储问题分开。服务器保存资源，客户端向服务器发送请求来操作这些数据。REST 系统必须按照这种客户机-服务器模型运行。

# **无状态**

这是 REST 风格最常见、最广为人知的约束，您可能已经意识到了这一点。它规定服务器不应该在调用之间保存客户端的任何上下文，并且从客户端到服务器的每个请求必须包含理解请求所必需的所有信息。

# **缓存**

响应必须将自己定义为可缓存或不可缓存，以潜在地提高可伸缩性。这里的重点是 HTTP 缓存，而不是服务器端缓存。像 [ETag](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/ETag) 这样不同的东西可以用于 HTTP 缓存。

# **分层系统**

在大型系统中，客户端不直接连接到终端应用服务器，可以有多个层，如负载平衡器、其他服务器等。介于两者之间。分层系统约束声明客户机不知道它是连接到终端服务器，还是连接到中间服务器。为了简化架构，这些层应该对客户端完全隐藏。

# **按需编码(可选)**

这是一个可选约束，规定服务器可以可选地向客户端发送代码以在本地执行/下载(例如，服务器发送 JavaScript 代码)。

# **统一界面**

REST 的核心特性之一是强调统一接口。在 REST 风格中，任何信息交换都应该在资源方面进行。服务器提供的任何可以命名的信息都是潜在的资源，如文档、图像、用户、帖子等资源集合。每个资源都由一个唯一的 URI 表示，客户端可以使用它来与资源进行交互。客户端还需要有一个资源的表示，它可以将它发送到服务器进行更新。例如，假设一个*用户*资源具有客户端可用的以下信息:

```
Representation: {
 “userId”: integer
 “userName”: string
 “email”: string
 “passwordHash”: string
}
```

`URI: [http://server.com/user/{userId](http://server.com/user/{userId)}`:这里 userId 应该是客户端发送的。
有了这些信息，为了更新 id 为 *1* 的用户的*用户名*，客户端可以首先使用 GET 调用获取该用户:

```
GET [http://server.com/user/1](http://server.com/user/1)
```

然后使用 PUT 动词将接收到的具有新的*用户名*的表示发送到同一个端点:

```
PUT [http://server.com/user/1](http://server.com/user/1) (HTTP request body should have the updated representation of the user).
```

在 REST 中，请求和响应必须是自描述的，这意味着接收方接收所有必要的信息来理解消息，例如，不能等待另一条解释如何解释数据的消息。通过使用标准方法(例如，正确的 HTTP 动词)发送 REST 消息来操纵资源，消息的接收者(服务器或客户机)接收所有必要的信息来完成其任务。

统一接口的另一个重要约束是 **HATEOAS** (作为应用程序状态引擎的超媒体)。该约束规定服务器响应消息必须提供相关资源的链接。这些资源链接根据客户端的当前状态为客户端提供其他当前可用的操作，这些操作在整个 API 中充当导航系统。例如，假设一个 API 端点用于通过 ID 获取帐户:

```
Request: GET [http://www.server.com/account/12345](http://www.server.com/account/12345)
Response body: {
  “account”: {
    “account_number”: 12345,
    “balance”: {
      “currency”: “usd”,
      “value”: 100.00
    },
    “links”: {
      “deposit”: “/accounts/12345/deposit”,
      “withdraw”: “/accounts/12345/withdraw”,
      “transfer”: “/accounts/12345/transfer”,
      “close”: “/accounts/12345/close”
    }
  }
}
```

在上面的响应中，有一些链接(如存款、取款等。)可以被客户用来对账户执行不同的动作，而不需要预先知道这些链接的结构。这主要是 HATEOAS 的本质。以 RPC 风格设计的相同 API 将产生以下 5 种不同的 API:

```
getAccountById(…)
depositToAccount(…)
withdrawFromAccount(…)
transferToAccount(…)
closeAccount(…)
```

在这种情况下，客户端需要知道所有公开的端点，并在其代码中与它们紧密耦合。

这是我想讨论的 REST 风格的六个主要原则。我希望这篇文章能够提高您对 RESTful APIs 的理解。我很想在下面的评论中知道你对这些休息原则的看法。

# 一些有趣的阅读

*   [https://stack overflow . blog/2020/03/02/best-practices-for-rest-API-design/](https://stackoverflow.blog/2020/03/02/best-practices-for-rest-api-design/)
*   [https://cloud . Google . com/blog/products/application-development/rest-vs-RPC-你想用你的 API 解决什么问题](https://cloud.google.com/blog/products/application-development/rest-vs-rpc-what-problems-are-you-trying-to-solve-with-your-apis)
*   [https://www . researchgate . net/publication/325770704 _ An _ Analysis _ of _ Public _ REST _ Web _ Service _ APIs](https://www.researchgate.net/publication/325770704_An_Analysis_of_Public_REST_Web_Service_APIs)—包含了一些对 REST APIs 的很好的分析，以及它们在多大程度上符合 REST 原则。