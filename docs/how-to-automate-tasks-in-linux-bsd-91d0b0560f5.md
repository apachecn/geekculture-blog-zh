# 如何在 Linux/BSD 中实现任务自动化？

> 原文：<https://medium.com/geekculture/how-to-automate-tasks-in-linux-bsd-91d0b0560f5?source=collection_archive---------8----------------------->

## 使用 crontab 自动化 Linux 中的任务

![](img/d501aa6cff819564a36fef10f7e41c89.png)

Photo by [Jason Leung](https://unsplash.com/@ninjason?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

大多数用户在计算机上执行的操作都是按需进行的。如果你想搜索信息，你可以在搜索栏中输入。如果您想要打开一个应用程序，那么您可以单击桌面图标来打开该应用程序。然而，某些行动需要定期的时间框架来有效地开展行动。

下面是如何在 Linux 和 BSD 上自动化任务。

## 什么是任务？

![](img/9767d80dd69276a9978412dfbeeeb079.png)

Photo by [Scott Graham](https://unsplash.com/@homajob?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

任务是需要运行的脚本。自动化任务基本上是一种定期运行任务的方式。某些作业需要自动任务才能正常执行。

一个很好的例子就是备份一台机器。您不需要在系统的每个状态改变时备份您的机器。因此，您可以运行一个脚本，将您的计算机自动备份到备份服务器。该脚本必须在每天午夜运行，以确保您拥有数据的每日副本。

在这里注册我的电子邮件列表。

## 您可以使用什么工具来自动化任务？

有一些基于命令行的 Linux/BSD 工具可以执行任务自动化。这是一些任务管理器应用程序。

*   [克朗](https://en.wikipedia.org/wiki/Cron)
*   [在](https://linuxize.com/post/at-command-in-linux/)
*   [系统定时器](https://linuxconfig.org/how-to-schedule-tasks-with-systemd-timers-in-linux)

最好的 CLI 工具是 cron。Cron 允许您在命令行中使用字符表示开始时间来调度任务。

## 安装 cron

在 Debian/Ubuntu 机器上

```
sudo apt-get install cron
```

在 Arch Linux 上

```
Installed by default with systemd/Timers
```

在 FreeBSD/OpenBSD 二进制软件包上

```
Installed by default
```

## 启用和启动 cron

您可以使用以下命令启用和启动 cron。

基于 systemd Linux 的系统

```
sudo systemctl enable cron.service
sudo systemctl start cron.service
```

在基于 FreeBSD 的系统上，您需要在 rc.conf 文件中添加下面一行。

```
cron_enable=TRUE
```

## 如何访问 cron？

您将需要访问 cron 菜单，以便。

```
crontab -e
```

然后你会看到一个如下的菜单。

![](img/a493c6156ab18a985af0799d6edfb22e.png)

Photo from Dre Chang

## Crontab 语法解释

每个任务的结构如下。

```
[minutes] [hours] [days of month] [month] [days of week] [task_script]
```

*   *:基于 systemd Linux 的系统上字段的所有值
*   ，:列表分隔符
*   -:范围分隔符
*   /:范围的具体步骤
*   @hourly:在每小时开始时运行
*   @daily:每天在 UTC 午夜运行
*   @weekly:在 UTC 每周日午夜运行
*   @yearly 或@annually:在 UTC 时间 1 月 1 日午夜运行

在这里注册我的电子邮件列表。

## 克朗塔布游乐场

 [## crontab . guru——cron 调度表达式编辑器

### cron 调度表达式的快速简单的编辑器我们创建了 Cronitor，因为 cron 本身不能…

crontab.guru](https://crontab.guru/) 

## 需要自动化的重要任务

有一些系统任务可以使用 cron 自动完成。

**备份服务器/台式机**

您需要将您的桌面和服务器备份到备份服务器，以确保您有数据的副本。建议您使用脚本来自动完成这一过程。该脚本包含可以来回传输和同步数据的备份和恢复命令。

[](https://github.com/JohnKaul/rsync_tmbackup) [## GitHub - JohnKaul/rsync_tmbackup:使用 rsync 的时光机风格备份。

### 这个脚本使用 rsync 提供了时间机器式的备份。它创建文件和目录的增量备份，以…

github.com](https://github.com/JohnKaul/rsync_tmbackup) 

您可以在 crontab 文件中添加以下命令，以备份位于用户个人文件夹中的内容。

```
# Starts rsync backup at 1am everyday
* */1 * * * if cat /proc/mounts | grep [mounted_backup_dir]; then cd [program_install_folder] && [script_file] /home [backup_location]; fi
```

**同步数据库**

同步数据库也是网络开发者将不得不面对的另一个问题。除非您使用自动化解决方案(如下所示)来创建可伸缩的数据库。

[](https://www.cockroachlabs.com/) [## 蟑螂实验室，建造蟑螂的公司

### CockroachDB 是一个分布式数据库，带有用于云应用程序的标准 SQL。CockroachDB 为公司提供动力，如…

www.cockroachlabs.com](https://www.cockroachlabs.com/) 

您将被迫定期同步数据库，以平衡来自计算服务器的流量负载。

IT 和工程领域是快速发展的领域。跟不上意味着你将被落在后面。跟上的最好方法是保持最新的新闻和教育内容。[订阅免费电子邮件列表，将你的职业生涯提升 10 倍。](/subscribe/@dretechtips)

**加入我们吧，因为 50 多位想要快速提升职业生涯和知识基础的人已经注册了。**

**相关内容**

*   [Linux 下如何安全删除文件？](/geekculture/how-to-securely-delete-files-in-linux-ce6ad1205922)
*   [如何在 Linux 中查找文件？](/geekculture/how-to-find-files-in-linux-6ed09a98c899)
*   [如何安全地连接到您的服务器？](/geekculture/ssh-securely-connect-to-your-servers-8895faab7083)