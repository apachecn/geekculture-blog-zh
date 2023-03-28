# 如果你用错了方法，Jayway Jsonpath 会消耗你大部分的服务 CPU。

> 原文：<https://medium.com/geekculture/jayway-jsonpath-can-consume-most-of-your-service-cpu-if-you-are-using-it-in-wrong-way-3230ba0d4407?source=collection_archive---------7----------------------->

![](img/fdb32e67794318214c484a1ab65d234f.png)

Photo by [riccardo ragione](https://unsplash.com/@rreason?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/cpu?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我有幸参与服务优化任务，并在优化过程中解决了几个问题，我认为这些问题对优化我们服务具有重要作用。

其中一个问题是 Jayway Jsonpath 库 CPU 消耗。Jayway Jsonpath 库使用了大部分服务 CPU，我们无法支持我们平台上想要支持的用户数量。

在深入研究并进行 cpu 分析之后，我们能够发现 Jayway jsonpath 库几乎使用了我们所有的 CPU。我已经写了关于如何使用你的工具包工具进行服务 CPU/内存分析的独立博客。如果你感兴趣的话，请看看。

Jayway jsonpath 库用于通过 json 路径访问 json 属性。

示例:-

Example code to read json attributes

在上面的示例代码片段中，我们提取了 json 属性“a”的值并打印出来。

在我们的项目中，Jayway Jsonpath 库的使用非常复杂。我们的服务充当中间件，调用其他第三方服务，并使用 JayJason 路径库将这些第三方服务的响应转换为我们自己的 Json 契约，并将响应返回给我们 API 的调用者。

我们以错误的方式使用了 Jayway Jsonpath，这导致了高 CPU 使用率。我们为每个读取的 json 属性解析 json 对象，而不是解析 json 一次，然后用它进行后续的属性读取。

在发现读取属性值的方法存在问题后，我们修改了代码，只解析一次文档上下文，然后用它来读取所有 json 属性值。

下面是示例代码，我们已经解析了一次 json 文档，并重用它来读取后续的 json 属性。

Parsing once and reading many times

通过这一代码更改，我们服务的 CPU 使用率下降到了 80%,我们能够支持我们想要支持的用户数量。

感谢阅读。
请👏或者关注我，如果你想从我这里读到更多。