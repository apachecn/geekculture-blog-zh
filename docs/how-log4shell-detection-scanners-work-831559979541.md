# log4 外壳检测扫描器如何工作

> 原文：<https://medium.com/geekculture/how-log4shell-detection-scanners-work-831559979541?source=collection_archive---------7----------------------->

## 通过静态分析进行检测

![](img/da1196274f033920e97734d2eab4e103.png)

H ello，🌎！Log4Shell 扫描器现在被广泛分发，用于检测易受攻击的 Java 归档(JAR)文件。如果您不知道什么是 JAR 文件，它们只是包含一组基于 Java 的类文件的 ZIP 压缩文件。类文件包含执行可移植 Java 代码的 Java 虚拟机(JVM)的可执行代码。在…