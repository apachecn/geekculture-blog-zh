# 在. Net6 中轻松验证

> 原文：<https://medium.com/geekculture/easy-validation-in-net6-7bb33ad4b6b3?source=collection_archive---------20----------------------->

您必须实现一种定制的方法来验证您的业务规则。我推荐使用 FluentValidations。但是，这是否让您感到头痛，或者您需要的不仅仅是 FluentValidations 提供的服务。这篇文章是给你的。

您可以使用设计模式进行验证。在这种情况下，我使用责任链。

**责任链**是一种行为设计模式，让你沿着处理程序链传递请求。收到请求后，每个处理程序要么处理请求，要么将请求交给链中的下一个处理程序。