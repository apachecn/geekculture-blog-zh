# 使用 Docker 进行远程 Java 应用程序剖析

> 原文：<https://medium.com/geekculture/yourkit-remote-java-application-profiling-with-docker-b2667eb88082?source=collection_archive---------14----------------------->

![](img/1e633551f96d5f3b097a4c5ced6b57f3.png)

Photo by [Caspar Camille Rubin](https://unsplash.com/@casparrubin?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/java-memory-profiling?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

对我来说，剖析 java 应用程序一直很棘手。与分析远程 java 应用程序相比，分析本地 java 应用程序更容易。

几天前，我在寻找分析远程 java 应用程序的方法，并花了相当多的时间来找出如何在不增加公司额外成本的情况下做到这一点。在寻找解决方案的过程中，我尝试使用了您的工具包工具，该工具允许您下载并免费使用 15 天，用于工具评估目的(我认为 15 天足以识别您的应用程序的问题😆).所以让我们开门见山，不要再浪费你的时间了。

## 要遵循的步骤。

*   使用命令
    **ssh 到您的 docker 容器主机，例如 ssh < docker 主机域或 IP >**
*   然后运行以下命令，列出 docker 主机上的 docker 容器
    ，例如 docker ps
*   然后 ssh 进入你的 docker 容器，用下面的命令
    访问 docker bash，比如 docker exec -it <容器 id 用 docker ps > bash 标识
*   然后，您需要在 docker 容器中下载您的工具包工具。运行以下命令下载并提取该工具。
    1。yum 安装 wget
    2。yum 安装 unzip
    3。wget[https://www . your kit . com/download/your kit-Java profiler-2021.3-b231 . zip](https://www.yourkit.com/download/YourKit-JavaProfiler-2021.3-b231.zip)
    4 .解压[your kit-Java profiler-2021.3-b231 . zip](https://www.yourkit.com/download/YourKit-JavaProfiler-2021.3-b231.zip)
    5 RM[your kit-Java profiler-2021.3-b231 . zip](https://www.yourkit.com/download/YourKit-JavaProfiler-2021.3-b231.zip)

一旦下载了您的 kit 工具和提取的内容，下一步就是识别您的 java 应用程序进程 id，并将其附加到您的 kit profiler 代理。要执行“在下面运行”命令

**sh your kit-java Profiler-2021.3/bin/attach . sh** 该命令将列出 docker 容器中运行的所有 Java 进程 id，并要求您选择要附加到 profiler 代理的进程 id。该命令的输出如下所示

```
Enumerating running JVMs with /home/YourKit-JavaProfiler-2021.3/jre64
[YourKit Java Profiler 2021.3-b231] Log file: /root/.yjp/log/profiler-ui-3035.log
Running JVMs:Name                             |   PID| Profiler Agent                  
-------------------------------- |------|--------------------------------
MyApplicationTwo                 |    82| Not loaded                          
Myapplication-19.3-fat.          |    13| Not loadedEnter PID of the application you want to attach (0 to exit) and press Enter:
```

现在输入要附加到探查器代理的进程 Id。一旦你输入了进程 id 并按下回车键，它将会要求你输入更多的内容，但是我建议你使用默认选项并按下回车键。

然后，您的 kit profiler 代理将开始在 **localhost:10001 上列出。**

以下是您可以在 Yourkit profiler 中使用的命令列表。

```
Usage: 
java -jar yjp-controller-api-redist.jar <host> <port> <command>where <command> is one of:status
capture-memory-snapshot
capture-hprof-snapshot
capture-performance-snapshot
start-sampling
start-async-sampling-cpu
start-tracing
start-call-counting
stop-cpu-profiling
clear-cpu-data
start-alloc-recording-all [alloc-sampled] 
  // record all objects
start-alloc-recording-adaptive [alloc-sampled] 
  // record all objects with size >= 4 KB, and only each 10th smaller object
start-alloc-recording=<N>,<B> [alloc-sampled] 
  // record all objects with size >= <B> bytes, and only each <N>-th smaller object
stop-alloc-recording
clear-alloc-data
print-alloc-object-count
  // get total count of all objects created during object allocation recording
print-alloc-object-size
  // get total size in bytes of all objects created during object allocation recording in full mode
start-monitor-profiling
stop-monitor-profiling
clear-monitor-data
start-stack-telemetry
stop-stack-telemetry
force-gc
clear-chartsExamples:
java -jar yjp-controller-api-redist.jar localhost 10001 capture-memory-snapshot
java -jar yjp-controller-api-redist.jar localhost 10001 start-sampling
java -jar yjp-controller-api-redist.jar localhost 10001 capture-performance-snapshot
```

现在，下一步是运行您认为有问题的用例，并捕获快照以使用您的套件应用程序进行分析。

> 运行你的用例并捕获快照，捕获快照的命令是
> **Java-jar your kit-JavaProfiler-2021.3/lib/yjp-controller-API-redist . jar localhost 10001 Capture-performance-Snapshot**

您的工具包将快照存储在/root/Snapshots/文件夹中。在您的工具包完成以下格式的快照后，您还将获得关于您的工具包存储快照的位置的信息。

```
 Snapshot captured: /root/Snapshots/MyApplicationOne-19.3-fat-2021-08-18.snapshot
```

现在，您已经准备好了快照，您需要使用 Yourkit 应用程序对其进行分析。为此，您需要在本地机器上下载快照，并将其导入到您的 kit 应用程序中。

按照以下步骤在本地计算机上下载快照。请注意“.”(句号)在命令结束时，这将在当前目录下复制快照。

> 1)将快照复制到您的 docker 主机
> docker CP<container _ id>:/root/Snapshot s/my application one-19.3-fat-2021–08–18 . Snapshot。
> 
> 2)将快照从 docker 主机复制到本地机器
> scp <主机 IP>:/home/nchinchore/my application one-19.3-fat-2021–08–18 . Snapshot。

现在您在本地机器上有了快照，现在您将需要您的工具包应用程序来分析快照。根据你的操作系统从这里下载。

[](https://www.yourkit.com/java/profiler/download/) [## 下载您的套件 Java Profiler

### 面向 Java EE 和 Java SE 平台的全功能低开销分析器。易于使用的性能和内存。网络…

www.yourkit.com](https://www.yourkit.com/java/profiler/download/) 

一旦你打开你的套件档案，它会问你激活密钥。您需要填写评估表，您将在提供的邮件中获得 15 天免费使用的评估密钥。

获得评估密钥后，添加该密钥并通过点击**打开快照导入快照…并开始分析。**

你可能会重复执行这些步骤，直到你发现问题。

```
1) **java -jar YourKit-JavaProfiler-2021.3/lib/yjp-controller-api-redist.jar localhost 10001** **clear-cpu-data**2) **Run your use case** 3) **java -jar YourKit-JavaProfiler-2021.3/lib/yjp-controller-api-redist.jar localhost 10001** capture-memory-snapshot4) **java -jar YourKit-JavaProfiler-2021.3/lib/yjp-controller-api-redist.jar localhost 10001** capture-performance-snapshot
```

调试愉快…😆！！