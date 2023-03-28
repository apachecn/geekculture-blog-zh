# Mule 4 —错误处理

> 原文：<https://medium.com/geekculture/mule-4-error-handling-551a4228748a?source=collection_archive---------0----------------------->

第一部分

![](img/1f9f841c60900674fecff0239286aeb7.png)

✦处理错误在处理开发 API 时非常重要。

✦错误可以定义为在处理计算机程序时发生的意外事件。为了响应这些异常，引入了**异常处理**。

> ***让我们来看看 Mule 4 中的错误处理****【✨】*

在 Mule4 中，我们可以使用 **On Error Continue** ，使用 **On Error Propagate** ，以及 **Raise error** 在流级别。

![](img/59ed802abbac0685885c7a70c184cc0a.png)

[https://www.ricston.com/blog/mule-4-error-handling-review/](https://www.ricston.com/blog/mule-4-error-handling-review/)

**自定义全局错误处理程序**和**默认错误处理程序**可以在项目级使用。

发生错误时，会创建一个错误对象。它有一些属性，如错误类型、错误描述..等等。

**✲错误类型** ➔是名称空间和标识符的混合体。

![](img/c31ce5e99faeaae4525c57583ff94947.png)

Mule 4 根据错误类型识别错误，然后指向相应的错误处理模块。

重要的✍️✍️✍️✍️

> ***错误只有在识别出该错误的具体错误类型被处理时，才会指向错误处理。***

*示例*:如果您得到一个 *HTTP: Not found* 错误，但是您正在处理 *Http: unauthorized* 错误，那么它将不会为您的错误处理块路由，因为它与错误类型不匹配。

## mule4 中的错误处理是如何工作的？

为了便于理解，让我们遵循下面的步骤。

☆首先，查看错误处理块中出现的任何内容。

如果是，查看特定的(错误传播或错误继续)**错误类型是否匹配**。

☆否则，mule 将使用其*默认错误处理*。

☆如果该流没有被任何其他流调用，那么它将显示错误响应中设置的默认值，并给出状态代码 500。(如果没有手动设置)但是如果您的流被另一个流调用，那么它将对特定的调用流产生一个错误。

☆如果在错误处理中存在某些东西，并且如果特定的错误类型匹配，

![](img/98e68e506b64a0aab63b524b1139b169.png)

让我们用下面的例子来理解这些步骤。

> ***例 1 :***

![](img/ad1d1f696b0950d59ee486476749a154.png)

这里，由于错误处理块中没有任何内容，它将转到默认的错误处理程序，并给出一个 500 状态代码，错误描述如下。

![](img/3e915d42c9d61d10c4316048262f771f.png)

> ***例二:***

![](img/9839eb830f1cd5f5f890f5c4d88e28dd.png)

因此，当错误处理部分中出现 on 错误传播并且错误类型也匹配时，它将转到 On 错误传播，并且将给出 500 状态代码以及错误处理块中出现的最新有效载荷。

![](img/167467d6c3f444356438f2f3a6400785.png)

> ***例题 3 :***

![](img/bf9f53c67f51839e80005b12ba6faadd.png)

根据示例 3，当错误处理部分中出现 On error continue，并且错误类型也匹配时，它将给出 200 成功状态代码以及错误处理块中出现的最新有效载荷。

![](img/04aacb2109aa786f84e165506bc62abb.png)

Output for Example 3

> ***例 4 :***

![](img/1af4cc190042d835c6913f568a9f9663.png)

根据示例 4，在错误处理部分中存在 on 错误继续和 on 错误传播，并且由于 On 错误继续中的错误类型不匹配，它将给出具有 500 状态代码的 On 错误传播块的有效载荷值。

![](img/749ad506657920fa641e8584e5a7a8d2.png)

*下面是 mule4 中错误处理的一些实例。*

> ***举例:***

![](img/afd46aa9fc96d7ef800479d38412ee59.png)

输出:

![](img/7be96105a061eada9a116c805a8202d7.png)

> ***举例:***

![](img/3cd850b7575ad99c7c48d4c5c1b1e31f.png)![](img/876d755d9fd4929351bd2b6bcb6e3716.png)

输出:

![](img/da40f220e4853ee3d019724378e75e1c.png)

> ***举例:***

![](img/9b62dbf222e225a607e9a4e137a10fd7.png)![](img/f1dc54449f579d97e111cbda0b1273f1.png)

输出:

![](img/222564aaeb8ce7a69e07038b31a0a4f9.png)

> ***例如:***

![](img/6a50c65e081dffe38c51aa22cec296c5.png)![](img/51a71468bb89366cef69ae3a8ef0a2ae.png)

输出:

![](img/8ae05e8f6a2e2a4a488e0202c51d15f2.png)

> ***举例:***

![](img/5a39c6d221f69a31ae1dff1e9d94a138.png)![](img/74e6307956d9f3ebe7313995c5b3f265.png)

输出:

![](img/abf9a72ec90b2ee7e60521d23347a591.png)

> ***举例:***

![](img/10af3b993b09cf8e6a5280e42222789f.png)![](img/4570dd44a1e4e354a2a74e2dd8f190f9.png)

输出:

![](img/6352306089a03bfdc3257fa3d0970940.png)

> ***举例:***

![](img/e8c33aed0f3291a22f36bd5840c43a90.png)![](img/367c2f1c58ff28ea7f5b1046ab02d150.png)

输出:

![](img/f8fb50f9055149a06ec1dbda6fdc6a38.png)

> ***举例:***

![](img/e3e4c065c56e20180dd8f245ebdee89c.png)![](img/f54241537ea062ad5b9a0c7a99bf5db2.png)

输出:

![](img/275ab9b61143f5b49af8d4d1e735d2d8.png)

> ***例如:***

![](img/ce7b376740afebeffadc40f6b0075f6b.png)![](img/3293e34b29d2d244f89e2c500dc8fa75.png)

输出:

![](img/45db4732c6c4c6f34e9b267d75185858.png)

> ***例如:***

![](img/b3fdf0641a2dda0c280b07d926074868.png)![](img/3e3f23484479b3d0a43faf96ce3f6d7d.png)

输出:

![](img/edaefba0d2abed90c781c03d30930226.png)

> ***例如:***

![](img/148072e0eb7126c55622046b2eb4eff6.png)![](img/8b5d80eb397ad61482e27f5a2cffc6b7.png)

输出:

![](img/c6a6f8a6b041b8d46db068a6ae336c97.png)

> ***举例:***

![](img/cd0c63cf04f81d35b5bdbbf243befbd9.png)![](img/8b5d80eb397ad61482e27f5a2cffc6b7.png)

输出:

![](img/869be0f212aa1c9f4ee9cd44e9098b33.png)

> ***举例:***

![](img/1d174c8068e9964dce467a03a854e8c2.png)![](img/d239b2db814ef754126ddeb34fccf11e.png)![](img/20497fa1d65066881e6a77323cdad169.png)

输出:

![](img/c57cee0b0917dcb362b8fb747496d9aa.png)

> ***举例:***

![](img/cf86cea0ead69d48aa2d34c85bf407a5.png)![](img/179bc10049f0a31e203fce5b4d7fdcb8.png)

输出:

![](img/c2968633290142d15a2adff7fe1e6159.png)

> ***举例:***

![](img/6f69a399de89ac22a6917f5ade4d625c.png)![](img/cfe1b390deaefc97c9b098205d39aa4c.png)

输出:

![](img/52e377a94c31969ff2a777e3e4199db6.png)

# 参考资料:

[](https://docs.mulesoft.com/mule-runtime/4.4/error-handling) [## 错误处理程序

### Mule 中发生的错误属于两个主要类别之一:Mule 抛出一个系统错误，当一个异常发生在…

docs.mulesoft.com](https://docs.mulesoft.com/mule-runtime/4.4/error-handling) [](https://blogs.mulesoft.com/dev-guides/how-to-tutorials/mule4-error-handling/) [## Mule 4 错误处理揭秘| MuleSoft 博客

### 像许多开发 API 和集成的开发人员和架构师一样，当我完成…

blogs.mulesoft.com](https://blogs.mulesoft.com/dev-guides/how-to-tutorials/mule4-error-handling/) [](https://blogs.mulesoft.com/author/sravanlingam/) [## Sravan Lingam | MuleSoft 博客

### Sravan Lingam 是 Virtusa 的 MuleSoft 大使和高级 MuleSoft 开发人员，拥有超过 5 年的 MuleSoft 经验。他…

blogs.mulesoft.com](https://blogs.mulesoft.com/author/sravanlingam/) [](https://docs.mulesoft.com/mule-runtime/4.4/intro-error-handlers) [## Mule 4 简介:错误处理程序

### 在 Mule 4 中，错误处理不再局限于 Java 异常处理过程，它要求您检查…

docs.mulesoft.com](https://docs.mulesoft.com/mule-runtime/4.4/intro-error-handlers)