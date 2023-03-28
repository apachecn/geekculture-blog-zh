# 从 MacBook M1 部署到 Heroku:Heroku CLI 或 GitHubActions

> 原文：<https://medium.com/geekculture/deploy-to-heroku-from-a-macbook-m1-heroku-cli-or-githubactions-868bc3a50935?source=collection_archive---------1----------------------->

安装一台新的笔记本电脑总是很有趣，但这次用我新的炫酷的 MacBook Pro 和苹果设计的 M1 芯片是一次不同的体验。该设备简直令人惊叹，性能非常好，但是，它也带来了一些令人头痛的问题，特别是对于需要运行各种工具的开发人员来说，这些工具还不兼容 ARM 架构。

这篇文章是关于如何在 Heroku 上部署(*)你在苹果硅 M1 上开发的项目:

*   使用 Heroku 命令行界面(使用 Rosetta)