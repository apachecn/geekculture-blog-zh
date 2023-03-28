# Heroku 上的多个码头工人图像

> 原文：<https://medium.com/geekculture/multiple-docker-images-on-heroku-3270ed31f2e5?source=collection_archive---------1----------------------->

## heroku.yml 和 Heroku Deploy Maven 插件的比较

在本文中，我将展示在 Heroku 上部署多个 Docker 映像的选项。遵循微服务理念，开发人员喜欢在独立的服务中划分职责，这些服务更容易开发、容器化和部署。

尽管 Heroku 对 Docker 的支持[令人敬畏](https://devcenter.heroku.com/articles/container-registry-and-runtime)，但是这个平台在处理多个容器时并没有达到它的标准。