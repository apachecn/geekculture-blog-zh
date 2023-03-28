# 如何在 BeagleBone Black 上执行 U-Boot 期间设置通用输入/输出(GPIO)

> 原文：<https://medium.com/geekculture/how-to-set-general-purpose-input-output-gpio-during-the-execution-of-u-boot-on-beaglebone-black-acd886307d49?source=collection_archive---------0----------------------->

![](img/002021bc31446cd0a74b4f3419edcb60.png)

在嵌入式程序执行的早期，有时您会希望启用某些硬件外设。

在操作系统运行之前，您的小型嵌入式设备需要花费大量时间来加载根文件系统。就在根文件系统建立之前…