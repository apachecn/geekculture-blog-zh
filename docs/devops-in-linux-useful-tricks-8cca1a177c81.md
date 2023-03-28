# Linux 中的 DevOps 有用的技巧

> 原文：<https://medium.com/geekculture/devops-in-linux-useful-tricks-8cca1a177c81?source=collection_archive---------0----------------------->

## Linux 中 DevOps 操作的一些技巧

![](img/1bfb43c2931cf0d439a78d9ea07203e1.png)

# 恢复删除的文件

如果您不小心删除了一个文件，并且该文件“幸运地”被一个进程打开，您可以使用`lsof (To find the process ID)`和`/proc/pid/fd/{id}`来恢复被删除的文件

```
# I have a file open for reading in one terminal
(T1 /tmp) $ less test.txt
This…
```