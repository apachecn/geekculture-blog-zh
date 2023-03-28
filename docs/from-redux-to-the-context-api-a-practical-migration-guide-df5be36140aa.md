# 从 Redux 到上下文 API:实用迁移指南

> 原文：<https://medium.com/geekculture/from-redux-to-the-context-api-a-practical-migration-guide-df5be36140aa?source=collection_archive---------63----------------------->

在我之前的帖子里， [**用 Redux 启动一个新的 App？首先考虑上下文 API**](https://auth0.com/blog/starting-a-new-app-with-redux-consider-context-api-first/)，我写了关于[上下文 API](https://reactjs.org/docs/context.html) 作为 [Redux](https://redux.js.org/) 的可行替代。在这篇文章中，我想展示一个使用 Redux 的 React 应用程序在使用上下文 API 时的样子。

# 首要考虑

我假设我以前的文章引起了您足够的兴趣，以至于您正在考虑从 Redux 迁移出去。你必须问自己:迁移值得吗？基于上下文 API 的方法可能更简单，但那不是…