# 用 NodeJS 中的 Ajv 验证您的 API 请求

> 原文：<https://medium.com/geekculture/validate-your-api-request-with-ajv-in-nodejs-7fba0ac0d2e8?source=collection_archive---------5----------------------->

放弃请求的手动检查。使用 AJV 自动化。

嘿伙计们，欢迎来到另一篇文章。今天我们将看看如何使用 AJV 来验证通过 ajv 进入 NodeJS 服务器的请求。

让我们从 AJV 的一些背景开始。简单地说，ajv 根据您定义的模式来验证您的传入请求。它为我们完成了最困难的部分，即使用您定义的模式验证复杂的请求体或查询参数。