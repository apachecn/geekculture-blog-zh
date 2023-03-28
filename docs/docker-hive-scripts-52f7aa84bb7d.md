# Docker 配置单元脚本

> 原文：<https://medium.com/geekculture/docker-hive-scripts-52f7aa84bb7d?source=collection_archive---------3----------------------->

## Docker 中 EMR 的助手脚本

我在这里分叉了 docker-hive 项目[https://github.com/alex-ber/docker-hive](https://github.com/alex-ber/docker-hive)基本上就是 EMR 5.25.0 集群单节点 hadoop docker 镜像。用 Amzn linux，Hadoop 2.8.5，Hive 2.3.5。

它模仿 docker 容器上 AWS 上的 EMR。你可以在这里阅读 Docker[https://github . com/Alex-ber/AlexBerDocs/tree/master/Docker/Windows](https://github.com/alex-ber/AlexBerDocs/tree/master/Docker/Windows)

我最近添加了一些 bash 脚本，我想在这里向您解释一下。

这篇文章似乎很长。它是用自上而下的方法编写的。建议至少看两遍。在第一次阅读时，所有的脚注都可以跳过，它们包含更详细的解释。在第二次阅读时，你可以选择阅读按钮向上。

# run-hive.sh

*该脚本旨在主机上运行。*

*   如果没有正在运行的 docker 容器，我们将运行一个。我们将等待它响应(见下面的 [checkisup.sh](#0d40) 部分)。
*   否则，我们将运行 bash 脚本。

```
bash run-hive.sh
```

默认情况下，它将是“reinit-HDFS . sh & & reinit-metastore . sh”。如果你愿意，你也可以明确地传递它们:

```
bash run-hive.sh reinit-hdfs.sh && reinit-metastore.sh
```

*   您可以将要运行的 bash 脚本作为参数传递。举个例子，

```
bash run-hive.sh reinit-hdfs.sh
```

威尔[只重新训练 HDFS](#1161) 。

```
bash run-hive.sh reinit-metastore.sh
```

will [只重新初始化 Hive Metastore](#b5ea) 。

# checkisup.sh

*预期使用示例:*

```
docker exec alex-local-hive checkisup.sh
```

其中 local-hive 是从 alexberkovich/docker-hive 映像创建的容器名称。[](#33cc)

使用案例:

*   当您(重新)启动 docker 容器时。如果无法连接到 Hive，您应该等待。
*   当您格式化 metastore 和/或(重新)启动配置单元服务器时。配置服务器做出响应需要时间。

基本上，这个 bash 脚本是一个繁忙的等待循环，它试图用 Beeline(连接到 Hive 的 CLI 工具)连接到 Hive 服务。它在睡眠中做了 10 次不同的尝试。在每次尝试中，它都等待来自 Beeline 的输出，如果输出还没有准备好，则有 10 个内部退休来读取输出(中间有一些睡眠)。如果成功，则返回代码 0。如果 10 次尝试后，连接仍未建立，则返回非零的返回代码。[](#1319)

# 重新初始化为空状态

您可以移除 docker 容器并创建一个新的容器，如下文 [](#33cc) 所述。这是可行的，但是有几个缺点:

1.  这要花很多时间。
2.  你会失去这个州。也许你有一些文件在 docker 文件系统上(不是 HDFS！)是您想要重用的。也许你想看看以前运行的日志，都将不复存在。
3.  也许对于您的用例来说，只[重新约束 HDFS](#1161) 或只[Hive Metastore](#b5ea)就足够了。
4.  也许对您的用例来说，重新约束 HDFS 和 Hive Metastore 就足够了，但是不要改动其他任何东西(例如 docker 的文件系统或与 HDFS 和 Hive 进程无关的文件系统)。

```
docker exec alex-local-hive bash -c "reinit-hdfs.sh && reinit-metastore.sh && checkisup.sh"
```

上面的代码片段将重新初始化 HDFS 和配置单元 Metastore，并将在配置单元服务器启动并可用于处理请求时返回。

**注:**

*   Docker 没有运行多个 bash 脚本内置功能。对于这种用例，建议使用“bash -c”。如果只有一个脚本‘bash-c’可能会被删除(假设 CMD/ENTRYPOINT 是/bin/bash 或/bin/sh，这是默认设置)。
*   你也可以用相反的顺序来做

```
docker exec alex-local-hive bash -c "reinit-metastore.sh && reinit-hdfs.sh && checkisup.sh"
```

它也是这样工作的，但是我认为上面的方法更具可读性。

*   如果你想重新约束 HDFS，只需运行:

```
docker exec alex-local-hive reinit-hdfs.sh
```

reinit-hdfs.sh 不影响配置单元服务器，所以可以从命令中删除 checkisup.sh。

*   如果您想[只重新初始化 Hive Metastore](#b5ea) ，您应该运行:

```
docker exec alex-local-hive bash -c "reinit-metastore.sh && checkisup.sh"
```

Apache Hive 将 Hive Metastore 和 Hive Server 捆绑在一起。所以，为了重新初始化 Hive Metastore reinit-Metastore . sh 脚本也重新启动 Hive 服务器。因此，您应该检查 Hive 服务器何时启动并可用于处理请求。

# reinit-hdfs.sh

*预期用途为:*

```
docker exec alex-local-hive reinit-hdfs.sh
```

其中 local-hive 是从 alexberkovich/docker-hive 映像创建的容器名称。[](#33cc)

使用案例:

*   您希望[从 Hive 中的空状态](#a734)开始。例如，为了运行单元测试，你可以在 2 个单元测试之间，格式化 HDFS。

***注:***

1.  此脚本不格式化 metastore。因此，您将看到 metastore 和 HDFS 不同步。参见下面的 [reinit-metasore.sh](#b5ea) 。
2.  在我的机器上，这需要大约 70 秒。在这个脚本运行完成后，你可以正常使用 HDFS。

基本上，我们停止所有的 HDFS 和纱线服务。我们格式化 namenode 并从 datanode 中删除所有数据，我们删除前一次运行的剩余数据，然后重新创建 HDFS 和 Yarn 服务，并重新创建 Hive 服务期望出现的文件夹。[](#5b0b)

# 配置单元 Metastore 注释

1.  Apache Hive 照常运行。它周围没有服务/bash 包装器。
2.  Hive Metastore 和 Hive Service 捆绑在一起。
3.  当配置单元服务第一次运行时，它创建配置单元 Metastore 表。
4.  如果我们想要格式化 Hive 的 Metastore，我们必须首先停止 Hive 服务。
5.  在 schemaTool 上重新创建 Hive 的 Metastore 并启动 Hive 服务的过程(它将创建版本表等)。这个脚本将这两件事包装在一起。

# reinit-metastore.sh

*预期用途为:*

```
docker exec alex-local-hive reinit-metastore.sh
```

其中 local-hive 是从 alexberkovich/docker-hive 映像创建的容器的*名称。[](#33cc)*

使用案例:

*   您希望在 Hive 中从空状态开始。例如，为了运行单元测试，你可以在 2 个单元测试之间，格式化 Hive 的 metastore。

***注:***

1.  此脚本不会格式化 HDFS。因此，您将看到 metastore 和 HDFS 不同步。参见上面的 [reinit-hdfs.sh](#1161) 。
2.  在我的机器上，这需要大约 36 秒。这个脚本完成运行后，您的 Hive 服务就可用了。

基本上，我们[停止 Hive 服务器，](#c79a)然后[初始化 metastore](#b5ea) ，然后[在创建 metastore_db 的模式下启动 Hive 服务器](#3bb6)。 [⁴](#7c17)

# init-metastore.sh

它是供内部使用的。

它删除 metastore_db 目录，并通过 schematool 运行 initSchema。它使用 Derby 作为存储。数据存储在 metastore_db 目录中。另请参见[配置单元 Metastore 注释](#9519)。

**注:**

*   当配置单元服务启动时，init-metastore.sh 在技术上可以运行，但应该避免这样做。
*   init-metastore.sh 不会创建版本表等(仅当配置单元服务正在运行时才创建)。

# stop-hiveserver2.sh

它是供内部使用的。

参见 [Hive Metastore 注释](#9519)。

基本上，我们使用 *ps* 实用程序和一些标识字符串来寻找 Hive 进程，而不是使用 *kill -9。⁵*

# start-hiveserver2.sh

它是供内部使用的。

参见 [Hive Metastore 注释](#9519)。

该脚本有两种运行模式:

*   没有任何参数。
*   带参数。

基本上，我们使用 *ps* 实用程序和一些标识字符串来寻找 Hive 进程。如果我们发现了，我们首先要阻止它。⁶

*   如果 start-hiveserver2.sh 不带参数运行，那么我们用**现有的 Hive Metastore 启动 Hive 服务器。**
*   如果 start-hiveserver2.sh 带参数运行(它旨在指示**创建 Hive Mestastore** ，但技术上它可以是任何值)，它将被传递到 hiveserver2。

# 功能. sh

*供内部使用。*

*pdate —* 打印当前时间的函数。

*echoerr* ， *echowarn* ， *echoinfo* —模拟记录器输出的功能。
*echinfo*向`stdout`发送消息(带有当前时间戳)。
*echowarn* ， *echoerr* 向`stderr.`发送消息(带有当前时间戳)

*killit* —以 process_id 为参数，向其发送 kill 信号。

如果失败，发送 kill -9。

如果仍然失败，它将退出并返回代码 1。

*findpid* —查找 process_id 的帮助函数。

它作为参数字符串，将在`ps aux`的 grep 中使用。

另外，也许你也会对我的 Git 教程[https://medium.com/@alex_ber/git-tutorial-40697ec6683f](/@alex_ber/git-tutorial-40697ec6683f)感兴趣

# 脚注:

创建 docker 容器的简单方法是从命令行运行以下命令:

```
docker run -p 8030-8033:8030-8033 -p 8040:8040 -p 8042:8042 -p 8088:8088 -p 10000:10000 -p 10002:10002 -d --name alex-local-hive alexberkovich/docker-hive
```

您可以在下面的简单脚本中编写它:

```
docker rm -f alex-local-hive && 
docker run -p 8030-8033:8030-8033 -p 8040:8040 -p 8042:8042 -p 8088:8088 -p 10000:10000 -p 10002:10002 -d --name alex-local-hive alex-docker-hive && 
docker exec alex-local-hive checkisup.sh
```

该代码片段将停止并删除现有的 docker 容器(如果存在)，将从 docker 映像创建 docker 容器，将启动 docker 容器，并将在 Hive 服务器启动并可用于处理请求时返回。

当然，你可以从源代码构建 docker 镜像，模式细节见[https://github.com/alex-ber/docker-hive](https://github.com/alex-ber/docker-hive)。

或者，您可以复制并粘贴以下 docker-compose.yml

**注意**:这两种方法在联网和日志记录方面存在差异。

在同一个目录中运行以下命令:

```
docker-compose up -d
```

-d 标志表示在*分离*模式下运行(在后台运行进程并立即返回 shell)。这是 docker exec 中的默认模式。

或者

```
docker-compose up -d --force-recreate
```

-force-recreate-后面的命令还将强制重新创建服务/容器。

此外，您可以编写一个简单的脚本:

```
docker-compose up -d --force-recreate && 
sleep 60 && 
docker-compose exec -T alex-local-hive checkisup.sh
```

此代码片段将删除现有的服务/容器(如果存在)，将启动 docker-compose，这将启动 docker-container，并将在配置单元服务器启动并可用于处理请求时返回。-T 标志表示不分配伪 tty(终端)。这是 docker exec 中的默认模式。

**注意**:两个 docker-compose 命令之间需要休眠，因为第一个命令会立即返回，服务可能不可用(例如，docker-container 尚未创建)。可以减少 60 秒。这是一个相当大的数字，因为启动 HiveServer2 需要时间，所以我们只是节省一些资源(而不是忙着等待，我们正在睡觉)。这种优化，但你需要一些睡眠。

**注意**:虽然上面的代码片段有效，但是您可以使用下面更有效的代码片段(您将获得大约 9 秒的时间):

```
(docker rm -f alex-local-hive || true) && 
(docker-compose kill alex-local-hive || true) &&
docker-compose up -d && 
sleep 60 && 
docker-compose exec -T alex-local-hive checkisup.sh
```

第一行删除 docker-container(如果找到的话)。秒链接删除 docker-撰写服务。第三行将启动 docker-compose，它将启动 docker-container，并将在 Hive 服务器启动并可用于处理请求时返回。-T 标志表示不分配伪 tty(终端)。这是 docker exec 中的默认模式。

**注意**:两个 docker-compose 命令之间需要休眠，因为第一个命令立即返回，服务可能不可用(例如，docker-container 尚未创建)。可以减少 60 秒。这是一个相当大的数字，因为启动 HiveServer2 需要时间，所以我们只是节省一些资源(而不是忙着等待，我们正在睡觉)。这种优化，但你需要一些睡眠。

在这里，我将更详细地描述 checkisup.sh:

首先，我使用 func.sh(如下所述)来记录活动。你会在`stdout`和`stderr`中看到发生了什么。如果你使用彩色 TTY，你会看到红色的`stderr`输出。

主循环最多进行 10 次尝试。每次尝试前(很少)睡觉。如果 10 次尝试失败，那么我们在`stderr`中有失败的指示，我们退出并返回代码 7。

我们在非交互模式下(无`stdin`)在*后台*运行 beeline 客户端，并且我们尝试在没有用户名&密码的情况下连接到 localhost:10000。我们将`stdout`重定向到 out_filename，将`stderr`重定向到 err_filename。

如果在启动 belling 时我们收到非零的返回代码，我们用相同的返回代码中止主循环。这在实践中应该是非常少见的。

我们有内部循环，运行多达 10 次尝试。每次重试前有几秒钟的睡眠时间。

*   如果我们在 out_filename 中找到了'*JDBC:hive 2://***'**，我们将把找到这个字符串的那一行打印到`stdout`，并退出，返回代码为 0。
*   如果我们在 out_filename 中发现'*直线* **'** ，我们打印到`stdout`直线还没有准备好，我们将进行另一次尝试(我们*继续*主循环)。
*   如果都没有找到，我们将当前重试尝试打印到`stdout`并进行重试(新的内部循环迭代)。

在进行新的尝试之前，我们检查 err_filename 中是否有“错误”字符串。如果找到，带有“错误”的行被打印到`stdout.`

如果内部循环超过 10 次尝试，我们将在`stderr`中得到一些指示，并返回代码 3。

如果主循环超过 10 次尝试，我们将在`stderr`中得到一些指示，并返回代码 7。

**限制:**

1.  错误文件名和输出文件名使用唯一的名称。这意味着如果这个脚本并行运行，将会出现竞争情况。一个可能的解决方案是将 UID 附加到名称上。
2.  暂停是在固定的时间内完成的(并且是硬编码的)。如果有另一个 Hive 客户端，它们也有这样的超时，我们可以在同一时间暂停。一种可能的解决方案是实现取幂回退。另一种方法是在一定的时间间隔内随机暂停一段时间。
3.  对特定字符串有硬编码的依赖性。如果配置单元版本将更改，则此类消息可能会更改，并且此脚本可能会失败。
4.  主循环(10)中的尝试次数和内循环(10)中的尝试次数是硬编码的。

在这里，我将更详细地描述 reinit-hdfs.sh:

**注意:**似乎 *stop-dfs.sh* 没有停止 namenode，所以我们手动完成。

*   我们正在停止 namenode 和所有其他 HDFS 的东西，我们停止纱(我们不希望有人会改变 HDFS 在此期间)。
*   我们手动删除 HDFS 数据节点，剩余的纱线等。：

/tmp/Hadoop-root-配置单元作业的 HDFS 根暂存目录。这里我们删除了 MapReduce 作业的剩余部分。

/tmp/hsperfdata_root —此目录是 Java 性能计数器的一部分。它是 JVM 在运行代码时创建的日志目录。`jps,` `jstat`和其他工具使用这个文件夹来避免连接到 JVM(例如，知道哪些 java 进程正在运行)。当所有 Java 进程停止时删除此文件夹，确保依赖于`jps` 工具的启动/停止脚本将正确工作。我不确定这一步是否 100%需要，但它是清理。

/tmp/*_resources —共享资源。当所有 Java 进程停止时，删除该文件夹是安全的。打扫卫生。

/home/hadoop/hadoopdata/是在 hdfs-site.xml 中设置的自定义值，它是 *dfs.namenode.name.dir* 和 *dfs.datanode.data.dir.* 的根目录。第一个参数确定 dfs 名称节点在本地文件系统上的哪个位置存储其数据。第二个参数决定了 DFS 数据节点应该在本地文件系统的什么位置存储其数据块。这确保了我们将有新鲜 HDFS，没有以前运行的剩余。

*   下一步是格式化 namenode。当所有 DFS 进程关闭时，我们可以格式化 namenode。它将重新创建命名节点元数据文件。
*   现在，我正在启动 namenode，因为 start-dfs.sh 不这么做。
*   下一步是启动所有与 DFS 相关的东西。
*   下一步，是开始与纱线相关的东西。
*   最后，我正在重新创建 Hive 服务器期望存在的空文件夹(权限为 777):
    /tmp
    /用户/Hive/仓库
    /tmp/Hadoop-yarn
    /tmp/Hadoop-yarn/staging

⁴在这里，我将更详细地描述 reinit-metastore.sh:

参见 [Hive Metastore 注释](#9519)。

首先，我们[停止 Hive 服务](#c79a)(基本上，我们为此使用 *kill -9* )。然后我们调用内部脚本 [init-metastore.sh](#b5ea) (基本上我们删除 metastore 数据并运行 *schematool -initSchema)* ，然后我们调用 *start-hiveserver2.sh* 带参数*' javax . jdo . option . connection URL = JDBC:derby:metastore _ db；create=true'* (基本上会[在创建 Metastore 的模式下启动 Hive 服务](#3bb6))。

⁵在这里，我将更详细地描述 stop-hiveserver2.sh:

参见 [Hive Metastore 注释](#9519)。

我正在使用 [func.sh](#d8e6) 内部脚本来 *findpid* 和 *killit* 。详细描述见 [func.sh](#d8e6) 。

1.  使用 grep 正则表达式中的 *ps* 实用程序和*"[j]ava[[:blank:]]-xmx 256m[[:blank:]]-DJ ava . net . preferipv 4 stack = true "*字符串查找 Hive 进程。
2.  如果没有找到这样的进程，我们将一些错误消息打印到`stdout`并退出，返回代码为 1。
3.  如果它找到了，我们将 process_id 作为参数，并向它发送 *killit* 函数(它基本上会发送 kill -9)。

**注:**

*   *[[:blank:]]* 表示 *' '* (空格字符)
*   [j]这里介绍 java 的诀窍[https://server fault . com/questions/27000/bash-regular-expression-question](https://serverfault.com/questions/27000/bash-regular-expression-question)

用于从匹配中删除进程*【grep Java】*。

**注:**

*   该脚本取决于配置单元服务器的启动参数如何反映在 *ps* 实用程序中。如果 *ps* 实用程序改变它显示参数的方式，或者参数本身将在未来的 Hive 版本中改变，这个脚本可能会中断。

⁶在这里，我将更详细地描述 start-hiveserver2.sh:

参见 [Hive Metastore 注释](#9519)。

我正在使用 [func.sh](#d8e6) 内部脚本来 *findpid* 和 *killit* 。详细描述见 [func.sh](#d8e6) 。

1.  使用 grep 正则表达式中的 *ps* 实用程序和*"[j]ava[[:blank:]]-xmx 256m[[:blank:]-DJ ava . net . preferipv 4 stack = true "*字符串查找 Hive 进程。
2.  如果发现这样的过程，我们首先要阻止它。
3.  如果 start-hiveserver2.sh 不带参数运行，那么我们用*' javax . jdo . option . connection URL =***JDBC:derby:metastore _ db***'*启动 Hive Server
4.  如果 start-hiveserver2.sh 带参数运行(本意是*' javax . jdo . option . connection URL = JDBC:derby:metastore _ db；****create = true****'*；但从技术上讲，它可以是任何东西)它将被传递到 hiveserver2。

**注:**

*   *[[:blank:]]* 表示 *' '* (空格字符)
*   [j]这里介绍一下 java 的窍门[https://server fault . com/questions/27000/bash-regular-expression-question](https://serverfault.com/questions/27000/bash-regular-expression-question)

用于从匹配中删除进程*‘grep Java’*。

**注:**

*   该脚本取决于配置单元服务器的启动参数如何反映在 *ps* 实用程序中。如果 *ps* 实用程序改变了显示参数的方式，或者参数本身在未来的 Hive 版本中也会改变，那么这个脚本可能会中断。