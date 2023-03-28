# 如何让您的定制对象属性匹配您的 JSON 键

> 原文：<https://medium.com/geekculture/three-amazing-strategies-that-you-wish-you-knew-to-make-your-custom-object-properties-match-your-7b45e6a90038?source=collection_archive---------16----------------------->

![](img/82227dc4619e900a7012a49777cf8f06.png)

Photo by [Anna Zakharova](https://unsplash.com/@annaazart?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

当使用使用 JSON 作为响应格式的 API 和服务时，你会注意到在大多数情况下它们不会遵循 [Swift API 设计指南](https://www.swift.org/documentation/api-design-guidelines/#general-conventions)。这意味着自定义对象的属性很有可能与从 API 返回的键不匹配。这个故事将教你三种策略，你可以用它们来匹配。