# 在 Swift 中解析 HTML/XML 中的字符实体

> 原文：<https://medium.com/geekculture/parsing-character-entities-from-html-xml-in-swift-4c6caed32db9?source=collection_archive---------7----------------------->

## 对于非 web 开发人员来说。

![](img/c410bbd99e48245ba54c9b1a6c29e455.png)

Photo by [Valery Sysoev](https://unsplash.com/@valerysysoev?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/html?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

今天的网络是文化、商业和技术的奇妙融合，新旧都有。作为一名 iOS 开发者，与 Web 的交互通常是琐碎的。从 web 服务器发出 REST API 的端点请求，获取数据，解码数据。嘣！完成了。至少，我是这么认为的，直到我在解析一个 xml 格式的响应时遇到了奇怪的子字符串，比如'"'和'&'。这些奇怪的文本称为字符实体引用(cer ),在这种情况下，分别代表引号( ")和&符号(&)字符。在本文中，我将提供一些背景知识，说明为什么 CERs 对我们这些非 web 开发人员来说是存在的，并给出一些在 Swift 中解码 CERs 的实用方法。

# 什么是角色实体引用，为什么存在？

正如我在文章开头提到的，cer 基本上是字符串中夹在&和分号之间的字符代码。您的 web 浏览器可以识别这些代码，并自动将它们替换为适当的字符，以呈现在您的屏幕上。所以，HTML 用' 5 < 6 渲染成' 5 < 6’. The Worldwide Web Consortium (W3) has a spiffy interactive [图表](https://dev.w3.org/html5/html-author/charref)如果你有兴趣看更多的可以看看。对于那些想深入了解的人，你可以阅读[维基条目](https://en.wikipedia.org/wiki/List_of_XML_and_HTML_character_entity_references)或者仔细看看[官方 html 规范](https://www.w3.org/TR/html4/sgml/entities.html)。

角色实体引用的存在有许多原因:

1.  允许在 HTML 中包含保留字符。就像任何编程语言一样，有些字符是为语言本身保留的……我们大多数人至少见过一点 HTML。每个标签都以< and ends with >开头。这些角色被保留下来有什么奇怪的吗？
2.  允许不包含在文档编码格式中的字符(现在 90%的网页都是 UTF-8，所以我认为这主要是针对边缘情况)。
3.  方便文档作者(web 开发人员)输入标准键盘上没有的字符。写'©'比在特殊字符库中搜索复制-写入符号要快得多，也更有效率。

# 在 Swift 中处理 cer

现在我们已经了解了背景，我将向您展示两种方法，您可以使用这两种方法在 Swift 中“查找和替换”cer。

# 选项 1:非属性字符串

如果我们深入了解 ObjectiveC(我知道，不太像 Swift)，`[NSAttributedString](https://developer.apple.com/documentation/foundation/nsattributedstring)`已经有很多围绕解析 html 构建的功能。下面是处理 cer 的`String`的另一个初始化器:

这里我们简单地初始化一个属性化的字符串，将`DocumentType`指定为`.html`。初始化时，字符实体引用会自动替换为适当的字符，所以我们所要做的就是返回`.string`属性，这样我们就完成了！新的初始化器可以像这样使用:

# 选项 2:正则表达式匹配

对于第二种技术，我们将编写自己的函数，使用 CER ->字符映射和正则表达式的字典来手动执行字符替换。

我们的字典会是这样的:

我已经省略了[完整列表](https://gist.github.com/jbadger3/7b386e7f284a96d8c8588fcbb459ddb5)，但是你已经明白了。至于实现的其余部分，让我们编写一个新的函数作为对`String`的扩展，这样角色子站在我们需要的任何时候都可用。以下是完整的代码:

那么这个函数在做什么呢？本质上，我们获取原始字符串，寻找与我们的`pattern`匹配的子字符串，迭代匹配，并通过使用在每个匹配中找到的范围替换任何 cer 来构建`result`字符串。对于那些不熟悉使用`[NSRegularExpression](https://developer.apple.com/documentation/foundation/nsregularexpression)`的人来说，有一篇由 Matt 在 NSHipster 上写的优秀的[文章](https://nshipster.com/swift-regular-expressions/)，提供了背景、例子和解释。当我把你从这篇文章引开的时候，我还应该推荐一下[regex101.com](https://regex101.com/)，一个我一直用来原型化正则表达式模式的交互式网站。

这个新函数可以在任何字符串上调用，如下所示:

# 结论

希望您会发现这些代码片段对您自己的工作有所帮助。感谢阅读！如果你觉得这篇文章有趣，点击下面的链接，当我发布新的东西时，你会得到更新。最后提醒，如果你还不是[媒体](/)的会员，请考虑[报名](https://jonathancbadger.medium.com/membership)。你将支持我(披露:我得到部分会员费)，并获得大量的伟大内容！

# 参考

*   【https://nshipster.com/swift-regular-expressions/ 
*   [https://www.w3.org/TR/html4/cover.html#minitoc](https://www.w3.org/TR/html4/cover.html#minitoc)
*   [https://gist.github.com/mwaterfall/25b4a6a06dc3309d9555](https://gist.github.com/mwaterfall/25b4a6a06dc3309d9555)
*   [https://www . swiftbysundell . com/articles/string-literals-in-swift/](https://www.swiftbysundell.com/articles/string-literals-in-swift/)