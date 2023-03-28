# 使用邮递员预请求脚本自动设置令牌

> 原文：<https://medium.com/geekculture/using-postman-pre-request-script-to-automatically-set-token-16fb978adc48?source=collection_archive---------1----------------------->

![](img/ee6c42778befaec0c8f611ff8b6c1e79.png)

Source ([link](https://softwareengineeringdaily.com/2020/06/30/postman-api-development-with-abhinav-asthana/))

在这篇短文中，我们将学习如何使用 [Postman 的预请求脚本](https://learning.postman.com/docs/writing-scripts/intro-to-scripts/)从 API 中获取访问令牌，并将其设置为环境变量，以便在进行实际 API 调用时可以使用它。

假设一个场景，您需要从`getUsers` API 获取用户，而`getUsers` API 需要认证。API 期望您设置访问令牌…