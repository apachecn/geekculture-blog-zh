# 使用自定义脚本监控 Linux 中的进程

> 原文：<https://medium.com/geekculture/monitor-processes-in-linux-with-custom-script-a6ceffc004d3?source=collection_archive---------17----------------------->

*当你能创造自己的工具时，为什么还要使用呢？*

![](img/c9653cc4fd9f02b32e451e05b4b2ce2a.png)

当您运行`px aux`时，我们可以看到多个状态。

```
USER PID %CPU %MEM VSZ RSS TTY STAT START TIME COMMAND
```

我们将通过读取 linux 系统中的`proc`文件来尝试从这里收集一些数据:

*   *在 Linux 中，每个进程都被赋予一个唯一的 PID。*