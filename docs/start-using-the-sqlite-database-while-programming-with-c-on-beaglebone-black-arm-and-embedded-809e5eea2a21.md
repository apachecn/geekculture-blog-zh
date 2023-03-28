# 如何在 BeagleBone Black 或其他 ARM 驱动的嵌入式设备上使用 C 编程时使用 SQLite 数据库

> 原文：<https://medium.com/geekculture/start-using-the-sqlite-database-while-programming-with-c-on-beaglebone-black-arm-and-embedded-809e5eea2a21?source=collection_archive---------5----------------------->

## 使用 C、ARM、Eclipse IDE 和 Linux 的 SQLite 实用介绍。

# 登陆

最近，我有一个任务，将一个 SQLite 数据库整合到一个打算在 Linux 和 Poky/Yocto 项目上运行的 C 应用程序中。Yocto 项目图像在 ARM 驱动的设备上运行。对于这个任务，我已经花了一些时间来制定一个实用的构建 SQLite 的分步指南…