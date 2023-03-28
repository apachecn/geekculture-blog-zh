# 使用 Elixir 编写服务器代码

> 原文：<https://medium.com/geekculture/coding-a-server-using-elixir-695565ed5e70?source=collection_archive---------4----------------------->

了解如何使用 Elixir 和 Plug 编写服务器代码。

![](img/b5b5314cf0c8863066f8117cbb8136e2.png)

在这个故事中，您将学习如何使用 Elixir 和 Plug 构建一个简单的服务器。

## 注意

如果你想更多地了解什么是 Elixir，以及 Elixir 和 Node.js 之间的区别，请务必查看我以前的故事。

[](/geekculture/what-is-elixir-3f6a96d8f642) [## 什么是仙丹？

### 什么是仙丹，为什么它很牛逼？

medium.com](/geekculture/what-is-elixir-3f6a96d8f642) [](https://javascript.plainenglish.io/a-2021-comparison-of-node-js-and-elixir-8744574033a7) [## Node.js 与 Elixir: 2021 对比

### 哪种语言是构建高性能和可伸缩后端的最佳选择？

javascript.plainenglish.io](https://javascript.plainenglish.io/a-2021-comparison-of-node-js-and-elixir-8744574033a7) 

开始吧！

# 为我们的服务器编码

在开始写代码之前，我通常会解释我们将要使用的技术和语言。

但是，我已经写了很多关于长生不老药的故事，所以我真的不认为我必须在这里解释长生不老药是什么。

## 创建新项目

首先，我们必须创建一个新的 Elixir 应用程序。只需运行下面的命令，但将项目名称改为您想要的名称。

```
mix new server --sup
```

我将我的应用程序命名为“服务器”。很有创意吧？哈哈。我说过，你想怎么叫都行。

我们使用`--sup`标志来创建一个受监督的应用程序。

## 添加依赖关系

我们将使用[和**插头。牛仔**](https://github.com/elixir-plug/plug_cowboy) 来构建我们的服务器。将它添加到您的应用程序中，如下例所示。

现在运行下面的命令来获取并安装库。

```
mix deps.get
```

就是这样！您已经添加了 Plug。牛仔到你的申请。

## 构建我们的路由器

为了处理请求和发送响应，我们必须制作一个路由器。您可以使用下面的示例来构建它。我马上会解释它是如何工作的。

在`lib/<your-app-name>`目录下创建一个`router.ex`文件，并在其中添加下面的代码。

如你所见，这其实很简单。

如果你向“/”发出 GET 请求，它会发回“Welcome”。但是如果你正在访问的网址不匹配任何路线，它会发回“没有找到”。

## 注意

您可以在这里添加任意多的路线。当然，您可以做的不仅仅是接收 GET 请求。你可以得到任何类型的 HTTP 请求，并做类似转发的事情。

我不会在这里展示如何做这类事情，但是如果你想了解更多，请务必阅读在线文档。

 [## 插头

### Plug is:不同 web 服务器的 web 应用程序连接适配器之间的可组合模块的规范…

hexdocs.pm](https://hexdocs.pm/plug/readme.html) 

## 配置我们的应用程序模块

为了能够运行我们的服务器，我们必须在我们的应用程序模块中配置它。

我不会在这里解释应用程序模块和管理器之类的东西，因为我计划为 OTP 写一个单独的故事。

把下面的代码加到`application.ex`文件里面就好了。

如果你不懂，也不用担心。我将在以后关于 OTP 的文章中更详细地解释这种东西。

## 运行我们的服务器

现在我们终于可以运行我们的服务器了！运行以下命令，并在浏览器中转至`localhost:4000`。你应该看到我们的“欢迎”信息。

```
mix run --no-halt
```

现在，尝试访问一个未知的 URL，比如`localhost:4000/settings`。您应该会看到“未找到”的消息。

# 最后的想法

您刚刚学习了如何在 Elixir 和 Plug 中构建服务器！请随意阅读这些文档，并自行改进这个项目。我说过，我很快就会写一篇关于 OTP 的文章。

希望这个故事对你有帮助。我很想听听你对这个故事的看法！

# 仅此而已。感谢您阅读这个故事！

如果你喜欢这个故事，一定要为它鼓掌！你想问我什么都可以。

在 Twitter 上关注我:

[](https://twitter.com/Re_allyedge) [## re _ ally 边缘

twitter.com](https://twitter.com/Re_allyedge) 

在 Patreon 上支持我:

[](https://www.patreon.com/allyedge) [## 阿里木阿尔斯兰卡亚是创造编程故事和教程。帕特里翁

### 今天就成为阿里木阿尔斯兰卡亚的赞助人:在世界上最大的…

www.patreon.com](https://www.patreon.com/allyedge)