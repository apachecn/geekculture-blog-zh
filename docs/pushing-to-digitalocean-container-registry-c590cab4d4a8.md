# 推送至数字海洋集装箱注册中心

> 原文：<https://medium.com/geekculture/pushing-to-digitalocean-container-registry-c590cab4d4a8?source=collection_archive---------13----------------------->

## 使用 Docker 和 DigitalOcean CLI 的 GitHubActions。

这是一个简短的教程，解释了如何在 GitHub 动作上构建图像，并将它们存储在 DigitalOcean Docker 注册表中。将展示两种方法(和样本`.yml`文件):使用 Docker CLI 和使用 DigitalOcean CLI。

![](img/f832a23f7e55f921cc9833dac1b04c9d.png)

Photo by [Luca Upper](https://unsplash.com/@lucaupper?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/freedom?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)