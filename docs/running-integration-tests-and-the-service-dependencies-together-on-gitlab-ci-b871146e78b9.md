# 在 Gitlab-CI 上一起运行集成测试和服务依赖

> 原文：<https://medium.com/geekculture/running-integration-tests-and-the-service-dependencies-together-on-gitlab-ci-b871146e78b9?source=collection_archive---------11----------------------->

![](img/10df63734673a5fbdb606aae56ff355f.png)

随着容器系统越来越受欢迎，我们有了更多的工具来拯救我们的生活。如今，几乎所有的 CI 工具都可以在一个隔离的容器中运行应用程序。以便我们能够在不同的环境中测试我们的代码。

因为在大多数情况下，在运行时，您需要连接一个真实的数据库或一些其他服务…