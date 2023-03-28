# 多国语言。网络 6

> 原文：<https://medium.com/geekculture/multi-language-with-net-6-3dd214ea01fb?source=collection_archive---------0----------------------->

您需要通过用户发送的请求来定义应该返回哪种语言的数据。为此，您可以使用请求头 Accept-Language。对于你收到的每一个请求，你都会有一个标题来知道用户想要的每一种语言。您可以通过 QueryString 获得该信息。在我看来，这不是一个好的标准。

# 实施

首先，让我们创建一个解决方案。在我的例子中，我选择了. Net6 的 API 模板。

```
<Project Sdk=”Microsoft.NET.Sdk.Web”>
```