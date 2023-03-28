# 将外部目录挂载到 Kubernetes 容器中

> 原文：<https://medium.com/geekculture/mount-external-directories-into-your-kubernetes-container-23ffe535bc4?source=collection_archive---------7----------------------->

![](img/b424c5022b910b38183bdf7240d2a6dc.png)

George Milton @ pexels

我想知道如何将卷挂载到您的 Kubernetes 集群中。

"为什么我必须在我的 pod 中安装一个目录？"你会问。

这个问题是正确的，但有几个原因，要么您将创建文档，并希望将它们放在共享驱动器上，以便其他服务或用户可以访问它们。再比如……