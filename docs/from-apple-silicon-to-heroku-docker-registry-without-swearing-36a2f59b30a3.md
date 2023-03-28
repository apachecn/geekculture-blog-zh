# 从苹果硅到 Heroku Docker 注册表没有骂人

> 原文：<https://medium.com/geekculture/from-apple-silicon-to-heroku-docker-registry-without-swearing-36a2f59b30a3?source=collection_archive---------3----------------------->

## 从 Mac M1 构建兼容英特尔 64 的 Docker 映像

Docker 桌面(原生)和 Heroku CLI(带 Rosetta)在苹果 M1 处理器上运行良好，然而，本地构建的 Docker 映像将无法成功部署到 **Heroku Docker 注册表**。

在之前的一篇文章[从 MacBook M1 部署到 Heroku:Heroku CLI 或 GitHubActions](/geekculture/deploy-to-heroku-from-a-macbook-m1-heroku-cli-or-githubactions-868bc3a50935) 中，我描述了如何使用 GitHub 操作构建 Docker 映像并将其部署到 Heroku。我在这里…