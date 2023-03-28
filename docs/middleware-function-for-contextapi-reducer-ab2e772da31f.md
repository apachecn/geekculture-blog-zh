# 上下文 API 中 Reducer 的中间件功能

> 原文：<https://medium.com/geekculture/middleware-function-for-contextapi-reducer-ab2e772da31f?source=collection_archive---------1----------------------->

## 用于实时状态更新的上下文 API WebSocket 中间件

![](img/175684cc553b24345645be22a56267b0.png)

Photo by [Ryan Song](https://unsplash.com/@ryansong?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/middle?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在上下文 API 中，创建 reducer 和初始状态对象来访问或更新来自其他组件的状态值。因此，`useReducer`钩子既用于获取状态值，也用于分派动作来更新状态提供者的值。在…的帮助下