# 立即开始您的脚本之旅| Bash 脚本—第 3 部分

> 原文：<https://medium.com/geekculture/start-your-scripting-journey-today-bash-script-part-3-9779ac0abea2?source=collection_archive---------4----------------------->

## 编写 Bash 脚本需要知道的一切

![](img/cfafbf920cdaa270f085d058a75d5deb.png)

Photo by [Daria Kraplak](https://unsplash.com/@daria_kraplak?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 以前的零件

**第 1 部分:** [立即开始您的脚本之旅| Bash 脚本—第](https://levelup.gitconnected.com/start-your-scripting-journey-today-bash-script-part-1-46cbddf4e4e7) 1 部分
**第 2 部分:** [立即开始您的脚本之旅| Bash 脚本—第](/geekculture/start-your-scripting-journey-today-bash-script-part-2-4d93ecb59249) 2 部分

## Crontab 概述

到目前为止，我们已经看到了如何编写 bash 脚本。在这一部分，我们将了解如何安排任务。在计划任务下，我们可以运行命令或脚本。

cron 实用程序是最受支持的任务调度实用程序。任何通过 crons 调度的任务都被称为 **cron 作业**。Cron 作业帮助我们自动化日常任务，无论是每小时、每天、每月还是每年。 **crontab** 是我们希望定期运行的命令列表。

要查看我们如何安排 cron 作业:

```
**cat /etc/crontab**
```

**示例:**

```
*“At minute 5.”****/5 * * * *** next at 2022-10-05 **20:05:00**
then at 2022-10-05 **21:05:00**---------------------*“At every 5th minute.”****/5 * * * *** next at 2022-10-05 **19:55:00**
then at 2022-10-05 **20:00:00**---------------------*“At 01:00 on every day-of-month from 1 through 7.”***0 1 1-7 * *** next at **2022-10-06 01:00:00**
then at **2022-10-07 01:00:00
...**
```

我们可以使用[**Cronitor**](https://cronitor.io/cron-job-monitoring?utm_source=crontabguru&utm_campaign=cronitor_top)**或** [**生成 cron 调度表达式**](https://www.generateit.net/cron-job/)

**重要命令:**

```
**crontab -l **           # List the jobs for the current user.
**crontab -r **           # Remove all jobs for the current users.
**crontab -e **           # Edit jobs for the current user.
**crontab -e** **-u <user>**  # Edit jobs for the specified user.
**crontab -l -u <user>** # List the jobs for the specified user.
**crontab -r -u <user>** # Remove all jobs for the specified users.
```

**添加作业:**

```
**$** crontab -e#add the following line to run the command every 5 minutes
***/5 * * * *** echo "testing" >> /home/admin/bash_script/cron.txt
```

在 crontab 中添加上述定义的作业后，每隔 5 分钟将向名为 **cron.txt.** 的文本文件中写入一个文本“测试”

## 任务— 1

假设，您必须监控一个 Linux 服务器。如果内存使用达到一定水平，您必须重启服务器。换句话说，如果空闲内存低于某个值，服务器应该重新启动。

现在，创建一个脚本来执行上述任务，并通过向 crontab 添加一个作业来安排拍摄。

**脚本:**

**添加玉米作业:**

```
**$** crontab -e #add the following line to run the script every 10 minutes
***/10 * * * *** /home/ec2-user/bash_script/reboot.sh
```

**列出所有工作:**

```
**$** crontab -l #add the following line to run the command every 5 minutes
***/5 * * * *** echo "testing" >> /home/admin/bash_script/cron.txt#add the following line to run the script every 10 minutes
***/10 * * * *** /home/admin/bash_script/reboot.sh
```

## 远程脚本执行

**基于密钥的 SSH 通信:** 假设我们要在两台服务器之间配置一个基于密钥的通信。为了更好地理解，我们将服务器命名为 server1 和 server2。

我们的目标是建立从 server1 到 server2 的基于密钥的 SSH 通信。

```
# Generate public key and private key for server1
**server1~ $** ssh-keygen# Share the server1’s public key with server2
**server1~ $** ssh-copy-id  -i  ~/.ssh/id_rsa.pub user@server2

# Check the connection
**server1~ $** ssh user@server2
```

**在服务器之间共享脚本** 如果我们想通过 SSH 连接执行脚本，那么我们必须事先与目标服务器共享脚本。

让我们使用 SCP(安全复制协议)与 server2 共享以下脚本。使用该脚本，我们可以检索相应服务器的磁盘使用信息。

**脚本分享:**

```
# Sharing script from server1 to server2
**server1~ $** scp ~/disk_update.sh   user@server2:~/tmp/
```

**从远程服务器执行脚本**

现在，是时候执行我们与 server2 共享的脚本了。但是我们将通过 SSH 连接从 server1 执行脚本。

```
**server1~ $** ssh user@server2 bash /tmp/disk-update.sh------------------ OUTPUT ----------------------
server2Total Used Use%
106687M 4947M 5%
```

## 任务— 2

想象一个环境，我们有数百台服务器。我们的任务是通过检索所有服务器的磁盘使用情况来创建报告。在这种情况下，我们可以编写一些脚本来从数百台服务器中检索磁盘使用信息。

**步骤 1:** 创建一个名为 **serverinfo** 的文本文件，并用用户名和相应的服务器 IP 或主机名填充它。
**例如:**管理员@服务器 1 或管理员@10.3.98.23

```
**serverinfo**admin@server2
superuser@server3
testuser@server4
...
...
```

**步骤 2:** 编写一个从服务器获取磁盘使用信息的脚本。

**第三步:**编写一个脚本，将上面创建的脚本执行到多台服务器上，并生成一个包含多台服务器磁盘使用信息的报告:

执行上述脚本后生成的报告—

```
**server1~ $ cat report.txt**-----------------------
server2Total Used Use%
133071M 7802M 6%
----------------------------------------------
server3Total Used Use%
65372M 1508M 3%
-----------------------
...
...
```

> *如果你觉得这篇文章很有帮助，请* ***别忘了*** *去点击* ***跟随*******拍拍*** *按钮帮我写更多这样的文章。* ***谢谢🖤****