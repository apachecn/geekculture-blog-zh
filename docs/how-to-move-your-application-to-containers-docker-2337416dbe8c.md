# 如何将我的应用程序移动到容器(Docker)？

> 原文：<https://medium.com/geekculture/how-to-move-your-application-to-containers-docker-2337416dbe8c?source=collection_archive---------19----------------------->

![](img/355be2d69be3ef624645823bcf954ecc.png)

Photo by [Bernd Dittrich](https://unsplash.com/@hdbernd?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在这篇博客中，您将了解如何构建容器映像，将应用程序迁移到 docker，以及部署容器和使用 Github 操作的各种方法。

如果您不熟悉容器[，请点击此处](https://sainadh086.medium.com/containers-a-way-for-building-and-deploying-applications-9061e9fc3e2d)

**简介 **

大多数人认为集装箱和码头是一样的。是的，他们只是在某种程度上是对的，当你深入这个世界，你会探索很多。

Docker 只是一个容器运行时引擎，作为一个客户端-服务器应用程序。Docker 使用 runC 对容器进行操作。

runC 是一个轻量级、可移植的容器运行时。它包括所有与 Linux 操作系统交互的底层代码。

Docker 使用这种技术来创建容器并相应地操作它们。随着 docker 变得如此受欢迎，我们在整个博客中使用相同的工具。

**选择正确的容器**

当您想要将应用程序移动到容器中时，您应该选择可以运行用于构建应用程序的编程语言的容器。

docker hub 中有许多容器，这是一个容器库，您可以在其中存储容器映像。有许多官方容器可用。还要选择语言版本。

```
# pull the container image from the docker hub
$ docker pull <container_name>:<tag>
```

**构建自定义容器映像**

为此，我们必须编写一个 docker 文件。许多容器运行时引擎都支持这个文件。

在这个文件中，我们必须编写一些基本的命令。

FROM:我们必须提到我们是在哪个基础容器上构建的。

运行:运行容器内部的命令。

ENV:用于编写应用程序所需的环境变量

复制:将文件从本地系统复制到容器中。

ADD:类似于 copy，用来复制压缩文件，直接解压到容器中。

WORKDIR:将任意目录设置为容器的根目录。

EXPOSE:通过隧道将内部端口连接到主机非常有用。用于通过应用程序的端口号向真实世界公开应用程序。

ENTRYPOINT:当您启动它时，它运行容器中的命令。

CMD:它类似于 ENTRYPOINT，CMD 可以在我们创建容器时更改，而 ENTRYPOINT 不能更改。

注意 Dockerfile 的文件名必须是 Dockerfile。

在这里，我用一个名为 url short ner 的小应用程序构建了一个 docker 图像，它将长 URL 转换为短 URL。

使用的编程语言是 python。

提取应用程序

```
docker pull python:latest
```

Dockerfile 文件

```
# building the Dockerfile
FROM python:latest# creating a directory for the application
RUN mkdir URL_SHORTNER # my dockerfile is in the same workspace as my application# copying the files from local system to container
COPY . URL_SHORTNER/#setting my working directory 
WORKDIR URL_SHORTNER#installing the required packages
RUN pip3 install requirements.txt#Exposing the application 
EXPOSE 8080# final command which run after container starts#here we write the command arguments in string format inside the list
CMD ["python3", "app.py"]
```

构建映像的命令

```
# inside the application workspace
$ docker build -t myapp:v1 .
```

如果您在创建容器后遇到任何问题，请检查您的容器日志。

```
$ docker logs <container_name>
```

现在将图像推送到容器存储库。

市场上有很多集装箱仓库，很少是 Docker hub、Quay、AWS 集装箱仓库等。

这里我使用的是 docker hub

```
# login to the docker hub using docker command$ docker login
```

输入上述命令的凭据。

要将图像推送到 docker hub 或任何其他容器 repo，它应该是特定的格式。

```
<container_repo_url>/<username>/<your_container_name>:<tag>
```

这里，container_repo_url 对于 docker hub 是可选的。

Docker hub: docker.io

Quay: quay.io

在 AWS 中，它将在您创建存储库时显示。

将映像推送到存储库的命令。

```
#attaching tag$ docker tag sainadh086/url-shortner:v1 myapp:v1#push the image to repo
$ docker push sainadh086/url-shortner:v1
```

**通过 Github 动作引入自动化。**

在向我们的代码中添加新特性之后，一次又一次地创建 docker 映像需要很长时间。

因为我们大多数人都在使用 git 这样的版本控制工具，并将代码存储在 Github 中。因此，将您的 Github 存储库与 Github 操作集成在一起。

在那里它自动构建容器并推送到存储库。

Github 还提供了一些预构建工作流来集成不同的应用程序。

正在写入 Github 操作文件。

```
name: CI #naming the pipelineon: **# select the branch and request type**
  push: **# selecting push command as the request**
     branches: [ master ]  # branch
  pull_request:
     branches: [ master ]jobs: **#multiple stages to run**
   build: **# it is one the stage named build****#selecting the os the instance to run our configuration file**
runs-on: ubuntu-latest     

      **# Multiple commands or tasks to run** 
      steps: - uses: actions/checkout@v2       - name: build a docker image 
         run: docker build -t infra:v1 . - name: pushing the image to docker hub
         **#here we require login credentials and we cannot show to everyone.
         # We use secrets feature provided by github.
         # here we read our secrets and pass them to the commands**shell: bash **#using bash terminal**
         env: **#passing the secrets to environmental variables**
           user_name: ${{ secrets.DOCKER_USER_NAME }}
           user_password: ${{ secrets.DOCKER_USER_PASSWORD }}you, guys run: | **#we use this symbol for writing multiple commands**
          docker login -u $user_name -p $user_password
          docker tag infra:v1 sainadh086/url-shortner:latest
          docker push sainadh086/url-shortner:latest
          echo "Success"
```

现在，您成功地构建了一个应用程序，然后将应用程序容器化，并将其移动到容器存储库中。现在，它已准备好进行部署。

选择您选择的云计算平台并部署应用程序。

在下一篇博客中，您将了解如何使用 terraform 将容器应用程序部署到 AWS 云中。

**结论**

您已经学习了一些关于容器的知识，将您的应用程序移动到容器中，将您的容器映像移动到 docker hub，以及将 Github 操作与 Github 集成。

如有疑问，请在下方评论。让我知道你想阅读什么类型的内容，或者你在使用 devops 工具时遇到了什么问题。

谢谢大家有美好的一天。

[](https://github.com/Sainadh086/URL-Shortener) [## GitHub-sainadh 086/URL-Shortener:这个项目用于缩短 URL。

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/Sainadh086/URL-Shortener)