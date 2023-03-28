# WireMock |使用 WireMock 和 Kotlin 创建您的第一个集成测试

> 原文：<https://medium.com/geekculture/wiremock-create-your-first-integration-tests-with-wiremock-and-kotlin-8814d5d00bb8?source=collection_archive---------8----------------------->

![](img/247ab58d6ceae63f9030aa272734974a.png)

Photo by [Steve Johnson](https://unsplash.com/@steve_j?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在本文中，我想谈谈用 Wiremock 模拟服务器响应。如何测试一个使用 http 请求获取数据并处理数据的函数或整个类？您需要一个实际的测试服务器，上面有实际的数据来测试它。该数据不应再更改，因为测试将依赖于它。这将是一个非常糟糕的解决方案，因为您需要为每个测试配置一个新的数据集，最终得到一个无用的服务器，上面有更多无用的数据。你能做什么？

# WireMock 就是答案。

*   如果您想用不同的输出测试不同的 http 请求，该怎么办呢？
*   如果您想测试相同的 url，但每次测试都有不同的输出，该怎么办？
*   如果您想指定一个 url 在所有测试中被全局调用时的行为，该怎么办呢？

WireMock 是所有这些问题的答案。

# 依赖关系

这些是我们在测试中需要的梯度依赖关系。注意到我还加了 [assertJ](https://assertj.github.io/doc/) 和 [khttp](https://khttp.readthedocs.io/en/latest/) ？这在测试时会很有用。

# 要测试的代码

这是要测试的代码。

1.  在这个类中，我们有两个方法，第二个方法从 Jenkins REST API 接收包含 HTML 文本的响应并返回它。它将 khttp 用于 get 请求。
2.  第一种方法使用参数，并将分支双重编码为 branchName。这是因为在 Jenkins 中斜线需要被编码。
3.  该方法调用 receiveResponse()函数。
4.  如果 statusCode 正好是 200，它将返回修订标记后的字符串，这是一个 git 散列。
5.  如果不是这样，它会打印出一个响应并抛出一个自定义异常。

测试整个类而不是其中一部分的唯一方法是使用 WireMock 进行集成测试。

# 整合测试

1.  首先，我们需要创建一个带有 TestInstance 注释的新测试类。
2.  创建带有动态端口的 WireMock 服务器。
3.  创建 Jenkins 类的对象
4.  启动服务器，并为本地主机和 BeforeAll 注释内的端口进行配置。
5.  为要测试 get 请求的 url 创建一个 stub。此 url 需要没有基本 URL ( [http://localhost](http://localhost.) )。
6.  它将返回状态为 200 的响应
7.  它的主体包含一个 HTML 文本，在我们的例子中，该文本在 Revision 子句后包含一个 git 散列。
8.  我们调用我们的方法来测试并将结果保存到 val 散列中。
9.  最后一部分是断言散列等于特定的 git 散列。

在这个测试中，我们将 WireMock 服务器配置为只在特定的测试中进行响应。我们还可以将 stubFor 放入 BeforeAll 注释中，并在每个测试中重用它。这将是一个例子…

有了这个工具，可能性几乎是无限的。

# 反射

## 下次我还会这样做吗？

是的，大部分是。我肯定会选择同样的嘲讽工具，但是像大多数情况下一样，我会多读一点它的文档。这将节省我一些时间来尝试让它工作。

## 什么进展顺利？

带有动态端口的 WireMock 服务器的创建比预期的要好。这真的很简单直接。

## 有哪些需要改进的地方？

下一次，我会在文档中读到更多关于我的特定用例的内容。大多数文章都是关于带有 Spring 框架的 WireMock，它提供了一些我无法使用的额外注释。我对 stubFor get()的定义有问题。我认为我需要使用前面带有 [http://localhost](http://localhost) 的整个链接，但是我没有。我费了很大劲才找到这个错误，但最终这个错误出现在了[文档](http://wiremock.org/docs/getting-started/#writing-a-test-with-junit-4x)中。

我希望这篇文章对你有用，感谢阅读😊