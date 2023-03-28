# 我如何构建一个过度工程化的无服务器运行时间检查器

> 原文：<https://medium.com/geekculture/how-i-built-an-overengineered-serverless-uptime-checker-cb6f49534356?source=collection_archive---------18----------------------->

最近我在做一个项目，我们与第三方合作处理不同的办公文件。这个第三方既有生产租户，也有开发租户，这在这些类型的集成中很常见。开发租户比生产租户更不稳定，至少感觉上，它会经常停机。在这一点上，这只是我们与服务的互动中的一种直觉，所以我们想量化它。当我们考虑如何检查服务是否中断时…