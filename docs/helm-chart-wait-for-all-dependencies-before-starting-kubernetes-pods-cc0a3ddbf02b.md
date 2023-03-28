# 舵图在启动 Kubernetes Pods 之前等待所有依赖项

> 原文：<https://medium.com/geekculture/helm-chart-wait-for-all-dependencies-before-starting-kubernetes-pods-cc0a3ddbf02b?source=collection_archive---------1----------------------->

## **通过支持等待依赖特性来提高你的舵图质量**

![](img/ce9f6dae3a83d0e9b0b9c28256bab564.png)

Photo by [NordWood Themes](https://unsplash.com/@nordwood?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

将微服务架构软件应用运行和部署到云平台或内部需要处理应用部署依赖性。这些依赖关系可能是一些第三方…