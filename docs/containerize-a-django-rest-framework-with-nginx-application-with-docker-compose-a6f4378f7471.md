# 用 Docker Compose 封装 Django (rest 框架和 nginx)应用程序

> 原文：<https://medium.com/geekculture/containerize-a-django-rest-framework-with-nginx-application-with-docker-compose-a6f4378f7471?source=collection_archive---------0----------------------->

![](img/2f17b178f851eb7253d6f1e13aa32ee7.png)

Photo by [frank mckenna](https://unsplash.com/@frankiefoto?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

所以在我们开始实际实现之前，我想解释一下 docker 是什么，以及它是如何带来好处的。

如果你在网上查过，你会发现 docker 有这些组件。

*   ***适用于 Mac、Windows 或 Linux 的 Docker****——它允许人们在各自的操作系统上运行 Docker 容器。*
*   ***Docker 引擎****——用于构建 Docker 镜像和创建 Docker 容器。*
*   ***Docker Hub****——这是用来托管各种 Docker 图像的注册表。*
*   ***Docker Compose****——用于定义使用多个 Docker 容器的应用。*

使用 docker，我们实际上实现的是一个可以在任何主机环境下工作的设置。因此，它可以消除我们面临的许多部署问题，比如在开发环境中工作的某些特定设置可能无法在生产环境中工作。

我们将只关注如何使用 nginx 作为反向代理将 docker 实现到 Django restful 应用程序中。django 和 docker 的任何安装都超出了本文的范围。

# 说够了，让我们实际做吧！

我们将创建两个 docker 文件

1.  用于 django 应用程序配置。
2.  对于 nginx 配置。

并将使用 docker compose 来创建两个可以相互协作的容器。

## 1.Django Dockerfile

这个文件需要放在 django 应用程序的根文件夹中。

Dockerfile contents for the django applifcation.

我们从 docker hub 获得 python:3.6.9-slim，并在/usr/src/app 中创建一个临时工作目录。

在我的例子中，它将安装所有像 mariadb 驱动程序这样的库，因为我在我的项目设置中使用 mariadb mysql。

一旦完成，它将复制 requirements.txt 文件并安装它，然后将 enire 项目源代码复制到/usr/src/app/。

现在我们实际上可以做 *docker build* 来构建 docker 文件作为 docker 映像，但是我们将使用前面讨论过的 docker compose 来完成。

## 2.Nginx Dockerfile 文件

它被放在项目根目录下的 nginx 文件夹中，因为这样很容易区分它们。

Dockerfile contents for Nginx.

在这里，我们从 docker hub 获得 nginx:1.17.4-alpine，删除 default.conf 文件并复制我们的 conf 文件。

nginx.conf file contents.

下面是我们如何将 nginx 作为一个反向代理。任何到达 nginx 容器的端口 8000 的流量都被提供给端口 8001，因为我们将在端口 8001 上提供 django 应用程序，您将在 docker compose 文件中看到它。

## 让我们把它们放在 docker-compose.yml 中

我们将在项目根目录下创建一个 docker-compose.yml 文件

docker-compose.yml file contents

在 docker-compose.yml 文件中有两个服务，如你所见，一个是 web，即 djanfo 服务，另一个是 nginx，顾名思义就是 nginx 服务。

我们将在根目录下构建 Dockerfile，也就是 django Dockerfile，正如您在上面的代码中看到的，我们使用 gunicon 作为 django 应用程序在端口 8001 上的应用程序服务器(直到我们使用 expose 命令公开该端口，nginx 才能访问该端口)。您拥有的任何环境文件都可以在 env_file 部分中给出。

现在，nginx 服务将使用我们已经创建的 nginx 文件夹中的 Dockerfile，并且容器端口 8000 被映射到我们将要部署的服务器的端口 80。

## 现在让我们变魔术吧！

当你跑步的时候

> docker-撰写构建

它将构建容器并

> docker-排版

将运行所有的容器，你现在可以检查你的 api 的工作。

为了辟邪

> docker-撰写向上-d

对于日志来说

> docker-编写日志

停止服务使用

> docker-向下合成

# 结论

万岁！我们终于完成了 django restful 应用程序的 dockerized，现在是庆祝的时候了。