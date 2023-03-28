# 在 Nginx 中检测移动用户代理

> 原文：<https://medium.com/geekculture/detect-mobile-user-agent-in-nginx-806a43f5782a?source=collection_archive---------4----------------------->

你可能会注意到，有些网站有两个不同的移动视图和桌面视图的网址，如脸书。m.facebook.com 负责移动视图，facebook.com 负责桌面视图，脸书[负责。通常，主要的网站，他们不关心响应式设计。如果客户希望在移动视图中很好地适应，只需使用另一个仅用于移动视图的网站，如 m.facebook.com](http://m.facebook.com)[(子域 m 表示移动)。](http://m.facebook.com)

在我看来，出于某种原因，为什么开发商或公司分开他们的网站响应，因为建立一个响应网站是相当痛苦的地方需要…