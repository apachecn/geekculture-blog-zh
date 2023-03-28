# 发出 XML 请求时如何在 Golang 中使用 CDATA 标签

> 原文：<https://medium.com/geekculture/how-to-use-cdata-tag-in-go-when-making-xml-requests-with-special-characters-23428570f840?source=collection_archive---------27----------------------->

![](img/d93ff7ae718d6b347b4fac51f87dfc0f.png)

Photo by [Cookie the Pom](https://unsplash.com/@cookiethepom?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/error?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我最近有幸与第三方集成了一个系统。他们有一个遗留系统，需要大量的 XML 请求来与他们的基础设施对话。我不是 XML 的狂热爱好者，但是在试图向他们的 API 发送未转义文本时遇到了一个问题。

> TLDR:我在请求结构上使用了 cdata 标记。(请随意跳到解决方案标题)

*本文的一个小警告。我假设您正在使用 Golang 中的 XML 模块，并且熟悉使用 marshaller 函数将请求转换为字节。这不是一篇关于如何发送 XML http 请求的文章*

# 短信

假设这个第三方系统被用来发送短信，我的短信看起来是这样的"*祝贺你中奖了。请回复领取您的奖品*。首先，我为我使用的糟糕的例子道歉。如果你收到类似上面的信息…我可能会忽略它。骗局预警！

无论如何，看看我的请求负载:

XML Request Structs

# 第一期

我遇到的第一个问题是第三方 API 不接受我的请求。我得到了一个“500”的响应和一个解组错误。经过一些尝试和错误，并删除了我的短信中的特殊字符，我能够发送一条成功的消息。太好了！我可以发送消息，但不幸的是这并没有解决我的问题。我的消息是动态的，可以在配置中更改。如果有人决定使用引号或双引号，我不能冒险破坏系统。

# 第一次尝试

我解决这个问题的第一次尝试是使用“html.EscapeString(s string)”对文本进行转义，这是由 Go 中的 html 库提供的，我希望第三方能够在另一端解码。好消息，消息发出去了，成功了。坏消息是，我的信息是这样的

"*恭喜你&# 39；你是赢家。请回复领取您的奖品*

因此，我们有一个工作的 API 请求，但不是我们想要的结果。然后，我发现从 Go1.6 开始，您可以使用‘CDATA’标记，但这确实会带来一些额外的开销。让我展示给你看。

# 解决方案

XML 对此有一个内置的解决方案，叫做 [CDATA(字符数据)](https://www.w3.org/TR/xml/#sec-cdata-sect)。它可以用于我们的 SMS 消息，因此我们可以包装我们的 SMS 消息，使它看起来像下面的`<！[CDATA[祝贺您中奖，请回复领取您的奖品]] >`。通过以下方式轻松完成:

Sprintf function for CDATA

但是…这感觉还是不太对，我想应该有更好的方法。Go1.6 添加了`,cdata`标签，要使用它，我们只需为我们的消息创建一个新的结构。这是最终的结构，点击[这里](https://pkg.go.dev/encoding/xml#Marshal)查看文档

Solution struct using cdata tag in golang

希望这能为你节省一点时间，因为我在这方面浪费了很多时间。如果你觉得这很有帮助，或者知道更好的方法，请给我鼓掌并留下评论。我尽量回复大多数信息

*通话完毕*

*克里斯*