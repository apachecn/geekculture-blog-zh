# 构建应用后端:第 2 部分

> 原文：<https://medium.com/geekculture/building-an-app-backend-part-2-a4c372f11de5?source=collection_archive---------55----------------------->

又见面了。在第一部分中，我们研究了如何使用负载平衡器和 kubernetes 服务来设置初始 API。这给了我们起步所需的基本 API 设置。现在我们来看看 API 背后的设计。

按照在线商店的主题，我们将处理一个订单。流程是这样的

*   顾客在商店下订单
*   在数据库中创建的订单
*   通过电子邮件通知客户订单确认
*   通知仓库包装…