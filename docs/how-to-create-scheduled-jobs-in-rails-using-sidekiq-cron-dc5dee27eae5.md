# 如何使用 sidekiq-cron gem 在 rails 中创建调度作业

> 原文：<https://medium.com/geekculture/how-to-create-scheduled-jobs-in-rails-using-sidekiq-cron-dc5dee27eae5?source=collection_archive---------5----------------------->

## 权威指南

![](img/44ac69f75996257c14a06e6a99c9204c.png)

首先，我们需要了解什么是计划作业。计划作业是那些被编写为在特定时间、日期或特定间隔定期运行的作业。他们也被称为克朗乔布斯。根据维基百科，cron 是类 Unix 计算机操作系统中基于时间的作业调度程序。

现在我们知道了什么是计划作业或 cron 作业。让我们继续讨论 [*sidekiq-cron*](https://github.com/ondrejbartas/sidekiq-cron) 。它是 sidekiq 的一个调度附件。这不是一个正式的 sidekiq 宝石。sidekiq 中的调度是一个企业级特性。如果你不想在那上面花钱，这个宝石是给你的。

![](img/bc00c148f816024e47c197027fc4c4aa.png)

所以，让我们开始吧。

首先，我们需要将 gem 添加到 rails 应用程序的`Gemfile`中。

每次添加新的 gem 时运行捆绑安装。

然后在*配置*目录下创建一个名为`*schedule.yml*`的文件。在该文件中，添加以下内容:

这里写着 5。这意味着它将每隔 5 分钟执行一次工作。您还必须将名为 ***WorkerName*** 的类改为对您的上下文有意义的类。

现在在你的应用程序`*config/initializers*`中创建另一个名为`*sidekiq.rb*` 的文件。将以下代码添加到该文件中:

现在是时候创建真正的工人了。为此，请转到终端并键入

```
rails g sidekiq:worker WorkerName
```

这将在应用程序的 app 目录中创建一个`*workers/worker_name.rb*`。注意，这里我使用 ***WorkerName*** 作为工人的名字。如果您之前修改过它，那么您现在也需要更改它。打开它，添加一些定期运行的代码。例如，它可以是对特定端点的调用，以检查是否有任何新值可用，如果有，则将其保存到数据库。(别担心，这只是个例子)

`*worker_name.rb*`文件如下所示:

最后一件事。拿着终端打字

```
sidekiq
```

这将在终端的一个选项卡中运行 worker。如果您想运行 rails server 或其他任何东西，请确保移动到另一个选项卡。因为工作人员需要在 app 运行的同时一直运行。

就是这样。您已经创建了您的第一个 sidekiq-cron 作业。呜哇！！