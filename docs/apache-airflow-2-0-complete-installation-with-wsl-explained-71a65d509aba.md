# Apache Airflow 2.0 Postgresql 完整安装及 WSL 说明

> 原文：<https://medium.com/geekculture/apache-airflow-2-0-complete-installation-with-wsl-explained-71a65d509aba?source=collection_archive---------3----------------------->

![](img/def098689942b2aa5fd1d86c6d1bb5b4.png)

A pache Airflow 是一个开源的 ETL 工具，它帮助**从源中提取**数据，然后**根据我们的需要转换**，最后**将**数据加载到目标数据库中。

*这是 ETL 的另一个优点(提取、转换和加载)*

我们可以根据我们的需求在气流中安排我们的 ETL 过程。

Apache airflow 是纯面向 python 的。

在 airflow 上的安装可能会很棘手，因为它涉及到需要设置的不同服务，例如对于并行处理，我们需要 PostgreSQL 或 MySQL 而不是 SQLite，即 airflow 处理元数据的默认数据库，这也是我们将讨论的内容。

我建议，如果有人对 docker 有基本的了解，最好使用 docker，而不是直接安装。

> [用 docker](https://kundansinghynr.medium.com/apache-airflow-2-0-complete-installation-with-docker-explained-f07aa1049f1e) 安装 Airflow 2.0

我们有两种方法来安装气流。第一个是 Docker，第二个是 WSL(Linus 的窗口子系统)。在这里，我们将安装气流与 WSL 如下。

**安装 WSL**

首先，我们需要知道什么是 WSL，WSL 代表 Linux 的 Windows 子系统。它允许我们在 Windows 内部使用 Linux 子系统。

安装 WSL 很简单，你可以去微软的官方网站，一步一步地在 Windows 中安装 WSL。

[](https://docs.microsoft.com/en-us/windows/wsl/install-win10) [## 在 Windows 10 上安装 WSL

### 安装 Windows Subsystem for Linux (WSL)有两个选项:简化安装(预览版)…

docs.microsoft.com](https://docs.microsoft.com/en-us/windows/wsl/install-win10) 

1.)以管理员身份通过 windows power shell 启用该功能。

```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

2.)重启你的系统。

3.)去 app store 安装你喜欢的 ubuntu。

现在，在 ubuntu 中设置好用户名和密码后，你将会看到一个终端。

![](img/959d14fc408c38f617339aeb4b9201b4.png)

如果你能在你的 ubuntu 上看到这个，你就可以安装 apache airflow 了。

第一步更新你的 ubuntu 系统。

```
sudo apt update && sudo apt upgrade
```

![](img/95d3be12d6c2b7d05dba63b33764f7b8.png)

接下来，更新 python3，安装 pip。

```
sudo apt upgrade python3
sudo apt install python3-pip
```

![](img/32abb9d15e8ff5140d4835f660283585.png)![](img/ab47a3fd915a368171d422e91f017759.png)

安装 PostgreSQL 不是必需的，但是如果我们要执行几乎每个 ETL 管道都需要的并行处理，我会推荐。

启动 PostgreSQL 服务。

```
sudo apt-get install postgresql postgresql-contrib
```

![](img/f5968ee47e416c351a94c4a5be9446c5.png)

启动 PostgreSQL 服务。

![](img/29c22289ee5a2a37e0cdeddd3df40cb2.png)

创建一个用户和一个 airflow 将要使用的数据库。

```
sudo -u postgres psqlpostgres=# CREATE USER airflow PASSWORD 'airflow';CREATE ROLEpostgres=# CREATE DATABASE airflow;CREATE DATABASEpostgres=# GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO airflow;GRANT
```

![](img/4506097f5726f2f61a3239c58c22a78e.png)

安装以下软件包，这是使用 PostgreSQL 进行 airflow connect 的先决条件。

```
sudo apt install libpq-dev
```

![](img/308e2526c9fe4f82ba2d4ed265ad4459.png)

以下命令是安装 airflow、PostgreSQL、psycopg2(用 python 连接 PostgreSQL)和设置 PostgreSQL 的路径。

```
pip install apache-airflow['postgresql']
```

![](img/1ef83769ad22efebfd8d32e016853ada.png)

```
pip install psycopg2
```

![](img/4fbb27847396df444db9d24b5746ca29.png)

```
export PATH=$PATH:/home/your_user/.local/bin/
```

![](img/3b543008442b655a66f26b78df8ea188.png)

现在，我们需要更改一些气流配置，以将气流与 PostgreSQL 连接起来，PostgreSQL 是可以在气流文件夹中找到的配置文件。

```
vim airflow.cfg
```

![](img/16f94714189e21b81ee86be0cbceef10.png)

*注意如果您在用户目录中找不到 airflow 文件夹，请键入 airflow scheduler，它将创建该目录，然后按 ctrl+c 取消。*

然后做以下修改。

```
executor = LocalExecutorsql_alchemy_conn = postgresql+psycopg2://airflow:airflow@localhost/airflow
```

这里，我们将顺序执行器改为负责并行执行的本地执行器。

apache airflow 元数据的默认数据库 SQL LIT 不能处理并行执行，因此我们将 SQL alchemy 连接改为 PostgreSQL。

![](img/26b3e687488ef84a32bc491052811e5b.png)

现在第一次初始化你的元数据库。

```
airflow db init
```

![](img/9f278bcdea6c007b02504f22abf530d2.png)

如果你看到一个长进程在你的系统中运行，没有问题，只需要查找最后一行初始化完成。

创建登录到 airflow web 服务器或 airflow UI 的用户。

```
airflow users create -r Admin -u admin -e admin@example.com -f admin -l user -p test
```

![](img/e95156050922e83eabe83c7fb601896d.png)

启动 airflow 服务器

```
airflow webserver
```

![](img/713b58f3f60bcfcc37e704667afcbfb9.png)

启动气流调度程序

```
airflow scheduler
```

![](img/c5e69a638400adf8f318f66b609da163.png)

在端口 8080 上检查您的本地主机，输入您创建的凭证，您就可以开始了。

![](img/1eb0e8d149896c92b98f85b609399406.png)![](img/09bd1e9bc98c31f0685c724802fa4635.png)

恭喜您，现在您已经成功地将气流安装到您的系统中。如你所知，现在安装 Apache airflow 并不容易，这就是为什么我们有 docker，其中的一切都已经设置好了。如果你想知道如何安装气流与 docker [点击这里](https://kundansinghynr.medium.com/apache-airflow-2-0-complete-installation-with-docker-explained-f07aa1049f1e)。

一些重要的参考资料，你可以看看。

[](http://airflow.apache.org/docs/) [## 证明文件

### Apache Airflow Core，它包括 web 服务器、调度程序、CLI 和最小气流所需的其他组件…

airflow.apache.org](http://airflow.apache.org/docs/) [](https://docs.docker.com/get-started/) [## 方向和设置

### 欢迎光临！我们很高兴你想学习 Docker。本页包含如何获得…的逐步说明

docs.docker.com](https://docs.docker.com/get-started/)