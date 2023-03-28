# 带有 OpenAPI 规范的 Mustache 模板

> 原文：<https://medium.com/geekculture/mustache-templates-with-openapi-specs-f24711c67dec?source=collection_archive---------0----------------------->

## OpenAPI 代码生成的最佳特性

本文在 OpenAPI 规范的背景下探索了 Mustache 模板，以及可以帮助开发人员优化代码生成的被低估的特性。

在 [Adyen](https://www.adyen.com/) ，我们前段时间采用了 OpenAPI 规范，我们创建了自己的 Mustache 模板，以便在每次有新的支付 API 发布时简化[Adyen SDK](https://github.com/Adyen)(各种语言)的生成。