# Azure Kubernetes 服务上的 Traefik 入口

> 原文：<https://medium.com/geekculture/traefik-ingress-on-azure-kubernetes-service-fa498ba7e4b4?source=collection_archive---------4----------------------->

如何在单个主机上公开多个服务

![](img/091a3e458472396a8ea14ad520eefdbc.png)

Photo by [Taylor Vick](https://unsplash.com/@tvick?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

将一个应用程序部署在由多个微服务组成的 Kubernetes 集群上，您可能希望公开其中的一些微服务，以便可以通过互联网进行访问。虽然这显然是为了您的 web 应用程序服务，但是您可能还想公开一些额外的 API。