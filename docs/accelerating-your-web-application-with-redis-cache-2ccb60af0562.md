# 使用 Redis 缓存加速您的 Web 应用程序

> 原文：<https://medium.com/geekculture/accelerating-your-web-application-with-redis-cache-2ccb60af0562?source=collection_archive---------7----------------------->

在本文中，我们将通过一个简单的例子演示如何使用 Redis 缓存加速 web 应用程序。我们将从 Redis 的一个小介绍开始。然后，我们将在 Node.js 中构建一个简单的 web 应用程序后端，为某个 Github 用户名检索 Github 存储库的数量。我们将测量 API 的请求-响应时间。之后，我们将实现一个 Redis 缓存中间件来存储检索到的数据并将其提供给用户，并将请求-响应时间与之前没有 Redis 的实现进行比较。