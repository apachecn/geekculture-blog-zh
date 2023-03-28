# 如何在 Linux 中将脚本作为服务运行

> 原文：<https://medium.com/geekculture/how-to-run-a-script-as-a-service-in-linux-4fb92cab6818?source=collection_archive---------7----------------------->

将 NodeJS 应用作为服务运行

> 系统服务简介

Systemd 是一个软件套件，它为 Linux 操作系统提供了一系列系统组件。它的主要目标是**统一跨 Linux 发行版的服务配置和行为**；“系统和服务管理器”是其主要组件。

它是第一个初始化引导序列的服务，并且始终以 PID 1 运行。

*   **init.d** 系统是 Linux 在启动时启动所有预期运行的服务和单元的第一个系统
*   较新的 Linux 系统已经切换到 system.d 来提供服务

这里有几个**系统的用例示例:**

```
# swithc to root:
sudo su -# list all the services
systemctl --type=service# start, stop and status of check tomcat service
systemctl status tomcat
systemctl stop tomcat
systemctl start tomcat-----------------------# list all the services
service --status-all# start, stop and status of check tomcat service
service tomcat status
service tomcat stop
service tomcat start
```

Systemd 服务还可以运行自定义脚本，这些脚本在系统启动时启动，并继续在后台运行。因此，在本文中，我们将设置一个脚本，在 Linux 服务器上运行 NodeJS 应用程序，作为服务运行。

![](img/7991d96a7a630c100bcb5a2d45c87ba5.png)

Image soruce: [itsfoss.com](https://itsfoss.com/start-stop-restart-services-linux/)

## 在后台运行 NodeJS 应用程序

因此，在我们设置脚本之前，先简单回顾一下上下文。传统上，我们使用以下代码运行 NodeJS 应用程序:

```
# using NPM
npm run start#using node
node your_app.js
```

这种运行方式有两个局限性:

*   不能在终端外运行或在您注销后运行
*   您不能在同一个终端会话中运行多个应用程序

您可以使用进程管理器在后台运行一个或多个 NodeJS 应用程序，甚至在您退出 Linux 服务器之后。PM2 是最新的 NodeJS 进程管理器之一，内置了负载均衡器。

> **对于本文的上下文，我已经使用 PM2** 运行了 NodeJS 应用程序和设置

这是一个我将用来运行我的 NodeJS 应用程序的脚本示例:

# 将 Node.js 应用程序作为服务启动

要设置脚本将 NodeJS 应用程序作为服务运行，请执行以下步骤:

在 **init.d** 目录中复制或创建您的脚本:

```
sudo su -
cd /etc/init.d
cp /location_of_myAppexec /etc/init.d# make sure your script is excutable
chmod +x myAppexec
```

现在，通过在 rc3.d 和 rc5.d 中创建符号链接，将该脚本复制到 RC 文件中:

```
ln -s /etc/init.d/myAppexec /etc/rc3.d/myAppexec
ln -s /etc/init.d/myAppexec /etc/rc5.d/myAppexec
```

> 注意:
> 
> 综上，rc。d 代表运行级别的“**运行命令**”，这是它们的实际用法。
> 
> rc1.d 到 rc3.d **当系统改变运行级别**时运行的脚本。运行级别 1 通常是单用户模式，运行级别 2 是多用户模式
> 
> rc3。d 用于**运行级别 3** 。所有这些目录都包含可执行脚本，这些脚本必须在 Linux 操作系统启动时启动。正如你所看到的，所有的脚本都是指向其他目录中的原始脚本的软链接

现在，您可以通过运行以下两个命令来启动、重启、停止和检查 NodeJS 应用程序作为服务的状态或日志:

```
# you can run on CI Tool like Jenkins when building and deploying
# new change and assure that your applicate is restartert with the  # fresh code while still running in the background/etc/init.d/myAppexec start# or on the server, you can check various state of your app 
# by simply run the following commands:service myAppexec status
service myAppexec restart
service myAppexec log
service myAppexec stop
service myAppexec start
```

> 如果你喜欢这篇文章，你可能也会喜欢； [Cron & logrotate:如何使用 Cron 自动化任务](/dev-genius/cron-logrotate-how-to-use-cron-to-automate-tasks-a93069d9185c)

> 干杯！！！