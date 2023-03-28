# 使用 Github 操作将 Docker 图像推送到 Amazon ECR

> 原文：<https://medium.com/geekculture/github-actions-pipeline-to-push-docker-images-to-amazon-ecr-4bca6ec864bd?source=collection_archive---------0----------------------->

## Github 操作管道将 Docker 图像推送到 Amazon ECR

![](img/c7914b4d4cf034e1d0185de67e0b4f0d.png)

在这篇短文中，我们将学习如何使用 [Github actions](https://github.com/features/actions) 构建 docker 映像并将其推送到 [Amazon ECR](https://aws.amazon.com/ecr/) 。

Amazon ECR 使您可以在任何地方轻松地存储、共享和部署您的容器软件。如果您有一个需要归档的应用程序…