# 通过 AWS 上的 Nginx 入口限制 Web 公共访问

> 原文：<https://medium.com/geekculture/restrict-public-access-with-nginx-ingress-on-aws-7fdcd0b6bed4?source=collection_archive---------1----------------------->

## 安全性

管理每个端点对 Nginx 入口控制器背后服务的公共访问

![](img/2ee2847a71deedb9db22d90efcbfe9fb.png)

Photo by [King's Church International](https://unsplash.com/@kingschurchinternational?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

保护对开发和测试环境的访问是创建这些环境时需要考虑的一个重要问题，尤其是在使用云原生工具和提供商时(这种情况非常普遍，而且很容易以公共环境告终)