# 气流、dbt 和 Postgres

> 原文：<https://medium.com/geekculture/airflow-dbt-and-postgres-841c37f107ba?source=collection_archive---------6----------------------->

![](img/024113dad37b0c8e7d85e8f93554b6e7.png)

Photo by [Ian Taylor](https://unsplash.com/@carrier_lost?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/docker?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 厌倦了在数据工程中为 PoC 重新创建 docker 映像？我为你创造了一个。

# 介绍

不知道你怎么样，但我很喜欢做兼职项目。通常，他们会测试不同的技术/工具，它们是如何交互的，以及我是否能在我的工作中使用它们。

对我来说，流程是创建一些 docker 映像或 docker-compose，如果它由交互应用程序组成，以拥有一个隔离和受控的环境。虽然我不是 docker 方面的专家，但有时创建环境占整个 PoC 工作的 50%以上。我运行一个 PoC，得到一些结果，很少将我的代码提交给 GitHub。几个月过去了，一个新的想法突然出现在我的脑海里；突然，我找不到我把代码放在哪里了。几乎每次我这样做，我都有同样的问题。

为了防止这种情况发生在我身上，我将为人们创建多个存储库，他们可以随意使用不同的工具和交互。

# 气流

我事实上的计划者。从 2019 年开始使用它，从一个简单的 dag 创建者开始，到与一个团队一起从 Airflow 1.10.X 迁移到 2.1.X 并应用最佳实践。当我在家的时候，你可以在我的博客上读到:

[](https://engineering.hometogo.com/apache-airflow-at-hometogo-8f97a12b2be5) [## HomeToGo 的阿帕奇气流

### HomeToGo 让数据流与气流协调一致的旅程。我们遇到的困难和问题以及我们如何应对…

engineering.hometogo.com](https://engineering.hometogo.com/apache-airflow-at-hometogo-8f97a12b2be5) 

我选择它是因为我是最熟悉的，但我以后不会只局限于此，所以还会有其他管弦乐队的资料库！

没有 Redis 和芹菜，我创建的气流图像会更加直观。我在做两台不同的机器，一台作为 web 服务器，一台作为调度器，当然还有后端 DB。

Dockerfile for Airflow

从 Dockerfile 可以看出，我用的是气流 2.4.2 搭配 Python 3.9。我不打算撒谎；我选择这个 Airflow 版本是为了稍后测试他们的数据集调度，并熟悉他们的新 UI。2.2.2 以后我就不查其他版本了。

代码本身非常简单。我正在安装多个包，即用于与 Postgres DB 交互的 libpq-dev、用于 dbt 的 git 和用于 python 包的 requirements。

我创建了两个 bash 脚本来控制调度程序和 web 服务器的启动行为。

scheduler entry point script

运行 dbt clean 和依赖项，如果数据库中不存在 data_warehouse DB，则创建它。初始化气流数据库并启动调度程序。

现在，在你走之前，审判我:

*   我从来不知道 bash——欢迎所有的评论；乐于改进和学习更好的实践
*   硬编码用户名、密码和 IP——目前不确定如何用另一种方式，我想我可以使用一些环境变量，并在所有地方添加它们，但是我已经在这里花费了比我希望的更多的时间

网络服务器更简单:

*   创建管理员用户
*   启动 web 服务器

因为我已经从存储库中挂载了 dags 文件夹—将您的 dags 放在那里，调度程序将解析它们。

# dbt

目前我转型的首选工具。安装相对容易——dbt-core 包+ dbt-DATABASE 包。

只有调整，使可持续发展，我写下了关于使用单独的配置文件。YAML 文件的凭据，为 PoC 不干涉任何签子的事情。我还添加了一个提示，关于它应该如何寻找 Postgres。

像创建 dbt 项目一样，在 transform/data_warehouse 文件夹中创建您的模型。

如果你对 dbt 感兴趣，你也可以看看我关于入门的帖子。

[](https://towardsdatascience.com/oh-my-dbt-data-build-tool-ba43e67d2531) [## 哦，我的 dbt(数据构建工具)

### 我的经验和使用这个超级工具一个月的一些注意事项

towardsdatascience.com](https://towardsdatascience.com/oh-my-dbt-data-build-tool-ba43e67d2531) 

# Postgres

为局部气流发展创造 docker-compose 并不是第一次；很快就能掌握什么在哪里。为什么我现在谈论气流？我把这个段落命名为 Postgres。我总是用 Postgres 作为气流的后端 DB。我打心眼里讨厌 MySQL，SQLite 没有并行性(对于 PoC 来说是可以的，但是为什么要等待多个任务完成呢？).因为我已经决定让它也成为我的数据仓库 DB，而不仅仅是作为气流后端，所以我必须做一些调整。尤其是使用 dbt。我必须创建一个包含连接信息(也包括凭证)的 YAML 文件。问题来了——不知道如何做到这一点，所以我从 stack overflow 和我在谷歌上找到的其他一些帖子中拼凑了这个美丽的东西。

这非常简单——用相关信息创建一个网络对象，然后在您的服务中使用它。

所以每次我启动 docker-compose 时，我总是在同一个 IP 和端口上获取我的数据库。

# 运行它

如果你熟悉 docker-compose，跑步也相对容易。

```
docker-compose build
```

来建立你的形象

```
docker-compose up
```

经营它。

# 摘要

总的来说，乐趣和愉快的经历使这一切运行。我了解了一些网络知识，以及如何让至少一些应用程序使用静态 IP 来模拟现实生活。

您可以找到这个存储库:

[](https://github.com/TomasPel/airflow_dbt_postgres) [## GitHub-to maspel/air flow _ dbt _ postgres

### 当运行一些 PoC 或尝试不同的工具如何一起玩时，通常我会创建 docker 映像或创建…

github.com](https://github.com/TomasPel/airflow_dbt_postgres)