# 使用 Azure Databricks 从日志分析中分析应用程序日志(Container Insight)

> 原文：<https://medium.com/geekculture/using-azure-databricks-to-analyze-application-logs-from-log-analytics-container-insight-75c102bde8bd?source=collection_archive---------12----------------------->

在过去的[故事](/geekculture/application-logging-of-java-spring-boot-containerized-app-in-aks-a7dc72959fb7)中，我分享了如何为 [Java Spring Boot 容器化应用程序实现日志记录，并将其提供给日志分析(容器洞察)](/geekculture/application-logging-of-java-spring-boot-containerized-app-in-aks-a7dc72959fb7)。这个故事是第 2 部分，我将分享如何从 Azure Databricks 执行日志分析。下面是环境设置的高级架构图。

![](img/06feaa7c701d7a3f61af2e634eb97d61.png)