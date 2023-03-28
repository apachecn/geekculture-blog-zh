# 如何从 AWS ECR 本地提取 Docker 图像

> 原文：<https://medium.com/geekculture/how-to-locally-pull-docker-image-from-aws-ecr-ebebbb4c100?source=collection_archive---------1----------------------->

![](img/8fc66ca2a8cd6a17e2541deb1e84634e.png)

Photo by [Rubaitul Azad](https://unsplash.com/@rubaitulazad?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/docker?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

如果您使用 AWS ECR 来托管私有 docker 映像，那么您可能需要将映像提取到本地进行测试或开发。

在这篇短文中，我将带您了解从私有 AWS ECR 存储库中提取 docker 图像的步骤。

# 步骤 1:安装 Docker