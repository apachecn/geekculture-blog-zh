# 不使用 Django Rest 框架构建 Django API 视图

> 原文：<https://medium.com/geekculture/building-django-api-views-without-django-rest-framework-4fa9883de0a8?source=collection_archive---------3----------------------->

几天前，我决定探索在没有著名的 Django Rest 框架(也称为 DRF)的情况下创建 Django API 视图的想法。不要误解我的意思，DRF 是构建 web APIs 的独特工具包，你应该尽可能多地使用它。这种工具包的不好之处在于，它们隐藏了底层技术的许多复杂性，人们无法了解它们到底是如何工作的。另一方面，我是一个喜欢摆弄这些技术的人，我决定为我的一个应用程序实现几个 Django API 视图。以便…