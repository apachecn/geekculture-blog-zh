# Linux — /proc/pid 目录第二部分

> 原文：<https://medium.com/geekculture/linux-proc-directory-part-two-1b59b7b404c1?source=collection_archive---------3----------------------->

## 深入研究 Linux /proc/pid 目录

![](img/6a55df077dba65b3d79b7ac2fd7e74e9.png)

`proc/<pid>`目录内容续:

# /proc/ <pid>/coredump_filter</pid>

从内核`2.6.23`开始，这个文件可以用来控制在内核转储文件中写入哪些内存段