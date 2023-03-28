# 我如何创建 Whatsapp REST API 和 CLI 来用 Golang 自动化我的消息(第 1 部分)

> 原文：<https://medium.com/geekculture/how-i-create-a-whatsapp-rest-api-and-cli-to-automate-my-messages-with-golang-part-1-3dd5b805462?source=collection_archive---------5----------------------->

![](img/98a0929aea06d08a82eba5f7351f3757.png)

Photo by [James Harrison](https://unsplash.com/@jstrippa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在过去的周末，我在寻找一些编码挑战，在我的搜索中，我发现了一个 [golang 库](https://github.com/Rhymen/go-whatsapp)，它允许我在扫描库生成的字符串二维码时连接，发送和接收消息，之后这个想法出现在我的脑海中！为什么不创建一个 REST API 来获取二维码，并创建端点来使用我的 WhatsApp 会话向任何号码发送消息？

# 尝试连接请求中的会话

我想到的第一个方法是创建端点，并在请求执行中运行并发 WhatsApp 连接，比如:

这是行不通的，我后来发现了这一点，因为响应是在 Whatsapp 连接关闭后编写的，但二维码是在执行时生成的，所以我想到了一种更大胆的方法来实现响应，而不是停止 Whatsapp 连接，在开发过程中，我创建了自己的 CLI 来自动创建样板项目，所以如果我想为 Whatsapp 连接创建 CLI 程序，速度非常快。 我认为这样一来，我可以使用 Whatsapp REST API 中的程序，并且在相同的情况下，CLI 将继续运行，假设 REST API 运行在 docker 容器上。

因此，我向您展示 Whatsapp 消息传递的 CLI:

【https://github.com/jsdaniell/whats-cli 

你也可以把它作为一个 npm 包来安装，或者直接使用它`npx`。如果你的机器上安装了 NodeJS，我使用 gorelease 来生成我将来会在 REST API 上使用的二进制文件，你可以很容易地用`npx whats-cli connect-qr`进行测试。您可以在[库](https://github.com/jsdaniell/whats-cli)上看到其他命令。

# 在 REST API 中使用 CLI

现在，我的 REST API 代码的主要变化是添加 CLI 二进制下载以使用请求控制器中的命令，所以在运行 API 之前，我的代码是这样的:

获取 Whatsapp CLI，现在只需要运行请求中的命令，并获取响应服务器的执行时输出，因此 CLI 程序将在执行 REST API 的 Docker 机器上继续该过程，我的 connect 函数现在是:

到目前为止，它只支持每个实例一个连接，但它只添加了一个参数来在 API 运行的 docker 映像目录上创建一个不同的文件 session.gob。

## 在本地测试 API

为了在本地测试 API，[克隆 git 存储库](https://github.com/jsdaniell/whats-api)，下载模块依赖项，并运行主文件。

`go mod download`

`go run main.go`

到目前为止，API 的端点有:

**获取** : `https://localhost:9000/getQrCode` >
*返回响应上的二维码 PNG。*

**获取** : `https://localhost:9000/disconnect` >
*断开服务器上实际登录的会话。*

**发** : `https://localhost:9000/send` >
*给某个号码发消息。*

```
{
 "number": "558599999999",
 "message": "Message"
}
```

[](https://github.com/jsdaniell/whats-api) [## jsdaniell/whats-api

### 这是一个 REST API 客户端，处理 jsdaniell/whats-cli 对服务器的 HTTP 请求，连接和发送消息…

github.com](https://github.com/jsdaniell/whats-api) 

我喜欢写我的存储库，这也是又一个学习自动化的周末，这是在开发中，但所有的工具都准备好了，现在是时候编写自动化工具的代码并撰写本文的第 2 部分了。