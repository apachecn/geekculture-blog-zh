# 如何对 Python 脚本进行 Dockerize

> 原文：<https://medium.com/geekculture/how-to-dockerize-a-python-script-b0af3cd11fb0?source=collection_archive---------2----------------------->

![](img/06ac5d321668f7c6598b38361a5b72e8.png)

Source ([link](https://towardsdatascience.com/docker-for-python-development-83ae714468ac))

在这篇简短的帖子中，我将带您完成一个简单 python 脚本的 Dockerizing 过程。有些情况下，您可能希望在 docker 容器中执行 Python 脚本。

例如，我们将定义一个简单的 python 脚本，它进行 API 调用并打印结果。让我们看看如何通过几个简单的步骤来实现它。

# 步骤 1:定义需求文件