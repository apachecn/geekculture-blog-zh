# Go & GitHub 工作流:自动化数据库相关的应用程序测试

> 原文：<https://medium.com/geekculture/go-github-workflows-automating-database-dependent-application-tests-4a79ee489848?source=collection_archive---------49----------------------->

最近，我开发了一个 Go 应用程序，其测试套件依赖于 MySQL 来运行。

本文演示了如何在 GitHub Actions 工作流中运行测试套件。

自动化 Docker 构建和推送过程，同时确保只有在测试通过的情况下映像才会被推送到 Docker Hub，这提高了我的部署管道的效率。

docker 文件的内容、一些 shell 脚本和工作流的配置(在一个名为`main.yml`的文件中定义)将是这里的重点。

我假设读者熟悉 Docker 和 GitHub 操作的基础。

也就是说，我是 GitHub Actions 的初学者，如果这些不是最佳实践，我会感谢更有经验的人的反馈。

**docker file**

docker 文件的第一步指示构建获取最新的 Golang 图像，并定义一些设置环境变量的构建参数。

```
FROM golang:latest as goLABEL author=”Joe Soap <joesoap@somedomain.com>”ARG DB_HOST
ENV DB_HOST=$DB_HOST
ARG DB_PORT
ENV DB_PORT=$DB_PORT
ARG DB_USERNAME
ENV DB_USERNAME=$DB_USERNAME
ARG DB_PASSWORD
ENV DB_PASSWORD=$DB_PASSWORD
ARG DB_DATABASE
ENV DB_DATABASE=$DB_DATABASE
ARG DB_CONNECTION
ENV DB_CONNECTION=$DB_CONNECTION
```

接下来，构建被指示创建一个项目目录，在那里安装 Revel 框架，然后将工作目录指向项目目录，并将项目的内容复制到那里。

```
RUN mkdir $GOPATH/github.com
RUN mkdir $GOPATH/github.com/…
RUN mkdir $GOPATH/github.com/…/…
RUN go get github.com/revel/revel
RUN go get github.com/revel/cmd/revel
RUN export PATH=”$PATH:$GOPATH/bin”
WORKDIR $GOPATH/github.com/…/…
COPY . .
```

接下来是安装项目的 Go 依赖项的指令:

```
RUN go list -m -u all
```

…以及向项目根目录中的两个 shell 脚本授予执行权限的指令:

```
RUN chmod +x ./start.sh
RUN chmod +x ./test.sh
```

最后一条指令运行`test.sh`脚本。如果测试通过，映像构建将完成(稍后将详细介绍):

```
RUN ./test.sh
EXPOSE 9000
CMD [“./start.sh”]
```

**外壳脚本**

这里的两个 shell 脚本是`start.sh` 和`test.sh`。

一开始，两个脚本都执行创建应用程序模式的命令(如果还不存在)，并运行任何未完成的数据库迁移以更新模式。

此后，`start.sh`将启动 API 服务，而`test.sh`将运行应用程序测试。`test.sh`仅在构建过程中运行(根据 Dockerfile `RUN ./test.sh`中的指令)。`start.sh` 是容器本身运行时运行(`CMD ["./start.sh"]`)。

在运行时，应用程序的容器通过主机网络访问主机操作系统的 MySQL 服务(稍后将详细介绍)。因此，测试需要反映这种行为。

`test.sh`的内容是:

```
#!/bin/bashecho “executing test shell”
go run command/main.go make-database
go run command/main.go migrate upecho “running tests”
if revel test -a . -v | grep -q ‘All Tests Passed.’; then
   echo “All Tests Passed.”
else raise error “API tests failed”
fi
```

前两个命令(`...make-database` 和`...migrate up`)创建和/或更新数据库。当在构建过程中执行时，总是会创建一个新的模式并使其保持最新。

第三个也是最后一个命令运行应用程序测试(`revel test -a . -v`)，如果测试命令的输出不是“所有测试都通过”，则通过抛出一个错误来评估结果在这种情况下，构建过程将失败，Docker Hub 上的`latest` 映像将不会被替换。

在工作流程中执行测试时，可以看到测试的输出:

![](img/d55b1746f490125f325b8b36e8684517.png)

Excerpt from a Workflow’s output

**main . yml**

为了简洁起见，将只详细讨论 **main.yml** 文件中向构建公开主机网络所必需的部分。

首先，`main.yml` 定义了工作流的触发器:

```
name: CI to Docker Hub
on:
 push:
  branches: [ master ]
 pull_request:
  branches: [ master ]
 workflow_dispatch:
```

接下来，它定义了工作流将要执行的作业(在本例中只有一个— `build`)。它还使用属性`runs-on`指定构建将在其中发生的主机操作系统(最新的 Ubuntu):

```
 jobs:
  build:
   runs-on: ubuntu-latest
```

`ubuntu-latest`默认安装了 MySQL，`build`启动服务的第一步和第二步分别是:

```
 steps:
    - name: Set up MySQL
      run: *|* sudo /etc/init.d/mysql start
```

…然后检查它是否处于活动状态:

```
 - name: Check MySQL Connection
     run: *|* mysql --version
      sudo apt-get install -y mysql-client
      mysql --host 127.0.0.1 --port 3306 -uroot -proot -e "SHOW DATABASES"
```

这里使用的密码(`-proot`)是“root”，因为这是 root 用户在`ubuntu-latest`上的默认 MySQL 密码。

在 MySQL 服务被启动和检查之后，该作业将检查存储库并使用 GitHub secrets⁴.登录到 Docker Hub

```
 - name: Check Out Repo
     uses: actions/checkout@v2 - name: Login to Docker Hub
     uses: docker/login-action@v1
     with:
      username: ${{ secrets.- }}
      password: ${{ secrets.- }}
```

下面的步骤使用 Docker Buildx，它将构建 Docker 映像。⁵

```
 - name: Set up Docker Buildx
     id: buildx
     uses: docker/setup-buildx-action@v1
     with:
      driver-opts: network=host 
```

在这里指定驱动程序选项`network=host`很重要，因为这将把主机操作系统的网络(以及它的 MySQL 服务)暴露给构建。

下面的步骤负责构建 Docker 映像并将其推送到 Docker Hub 的`latest`标签下。它使用 Docker 构建和推送操作来实现这一点:

```
 - name: Build and push
     id: docker_build
     uses: docker/build-push-action@v2
     with:
      context: ./
      file: ./Dockerfile
      network: host
      allow: network.host
      build-args: *|* DB_HOST=localhost
       DB_PORT=3306
       DB_PASSWORD=root 
       DB_USERNAME=root
       DB_CONNECTION=mysql
     push: true
      tags: ${{ secrets.- }}/someproject:latest
      cache-from: type=local,src=/tmp/.buildx-cache
      cache-to: type=local,dest=/tmp/.buildx-cache
```

在`with:`属性下指定`network:host`和`allow: network.host`很重要。

我们还应该注意这里的构建参数(`build-args:`)。这些在 Dockerfile 文件中定义(见上文)，并利用主机操作系统的 MySQL 凭证作为映像的环境变量(`localhost`是主机操作系统的网络)。MySQL 将在`ubuntu-latest`中的默认端口 3306 上运行。

当`test.sh`脚本在这个步骤中运行时(见上文)，如果任何测试失败，构建都不会完成。

最后两步将图像摘要回显到终端并运行 layer-caching⁶(分别为):

```
 - name: Image digest
     run: echo ${{ steps.docker_build.outputs.digest }}

   - name: Cache Docker layers
     uses: actions/cache@v2
     with:
      path: /tmp/.buildx-cache
      key: ${{ runner.os }}-buildx-${{ github.sha }}
      restore-keys: *|* ${{ runner.os }}-buildx-
```

以下是完整的`main.yml`文件:

```
name: CI to Docker Hub
on:
 push:
  branches: [ master ]
 pull_request:
  branches: [ master ]
 workflow_dispatch:
  jobs:
   build:
    runs-on: ubuntu-latest
    steps:
     - name: Set up MySQL
       run: *|* sudo /etc/init.d/mysql start - name: Check MySql Connection
       run: *|* mysql --version
        sudo apt-get install -y mysql-client
        mysql --host 127.0.0.1 --port 3306 -uroot -proot -e "SHOW DATABASES" - name: Check Out Repo
       uses: actions/checkout@v2 - name: Login to Docker Hub
       uses: docker/login-action@v1
       with:
        username: ${{ secrets.- }}
        password: ${{ secrets.- }}

     - name: Set up Docker Buildx
       id: buildx
       uses: docker/setup-buildx-action@v1
       with:
        driver-opts: network=host - name: Build and push
       id: docker_build 
       uses: docker/build-push-action@v2
       with:
        context: ./
        file: ./Dockerfile
        network: host
        allow: network.host
        build-args: *|* DB_USERNAME=root
         DB_PASSWORD=root
         DB_PORT=3306
         DB_HOST=localhost
         DB_CONNECTION=mysql
       push: true
       tags: ${{ secrets.- }}/someproject:latest
       cache-from: type=local,src=/tmp/.buildx-cache
       cache-to: type=local,dest=/tmp/.buildx-cache

     - name: Image digest
       run: echo ${{ steps.docker_build.outputs.digest }} - name: Cache Docker layers
       uses: actions/cache@v2
       with:
        path: /tmp/.buildx-cache
        key: ${{ runner.os }}-buildx-${{ github.sha }}
        restore-keys: *|* ${{ runner.os }}-buildx-
```

**重述**

如果您需要在 GitHub 工作流程中让 Docker 构建过程可以使用主机操作系统的网络，这是一种方法。

用户也可以在 Docker 构建期间运行 shell 脚本。该脚本可以创建一个最新的模式(通过运行应用程序使用的任何数据库版本控制命令)，以及运行依赖于该模式的任何应用程序测试。

要做到这一点，必须确保主机的网络暴露在构建过程中。这是通过以下方式实现的:

1.  在 Dockerfile 文件中使用适当的构建参数；
2.  配置 GitHub 工作流，以便将主机网络暴露给构建；而且，
3.  声明包含主机操作系统的 MySQL 凭证的构建参数，并通过`build-args`属性在工作流的构建步骤中设置它们。

感谢您阅读本文！

[1]: [https://bludog.app](https://bludog.app)

[2]:在 Go 项目中使用框架是有争议的。许多开发人员认为它们是不必要的抽象。我对这种批评很敏感。这个项目很少使用 Revel。它有自己的 ORM (Revel 不包含 ORM)、数据库版本控制系统、作业调度程序和响应处理程序。无论如何，框架的使用与本文中概述的技术没有关系，这些技术可以在任何依赖于数据库的测试中实现，这些测试可以从命令行运行。

[3]:我决定不对这个项目的数据库进行容器化；一般来说，我宁愿避免这样做。

[4]:参见[https://docs.docker.com/ci-cd/github-actions/](https://docs.docker.com/ci-cd/github-actions/)了解这些步骤的更多细节

[5]:关于 Docker Buildx 的更多细节，请参见[https://docs.docker.com/buildx/working-with-buildx/](https://docs.docker.com/buildx/working-with-buildx/)

【6】:[https://github.com/actions/cache](https://github.com/actions/cache)