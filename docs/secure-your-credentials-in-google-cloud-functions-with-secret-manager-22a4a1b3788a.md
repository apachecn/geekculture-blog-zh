# 使用 Secret Manager 保护您在 Google Cloud 功能中的凭证

> 原文：<https://medium.com/geekculture/secure-your-credentials-in-google-cloud-functions-with-secret-manager-22a4a1b3788a?source=collection_archive---------7----------------------->

![](img/e2a67ac8bb070132ba57269dfddc4550.png)

Image [source](https://cloud.google.com/secret-manager)

我们知道在源代码中硬编码凭证是不安全的，但是在环境变量中使用它们是不安全的，因为第三方依赖项或库在运行时可以访问这些变量。此外，使用某些框架会导致环境变量的内容被发送到日志中。