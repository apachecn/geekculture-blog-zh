# 使用 C++语言在嵌入式 Linux 和 ARM 驱动的设备中处理 QR 码(动态链接)

> 原文：<https://medium.com/geekculture/working-with-qr-codes-in-embedded-linux-and-arm-powered-devices-using-the-c-language-55d111b51aea?source=collection_archive---------6----------------------->

关于在运行嵌入式 Linux 的 ARM 驱动设备上使用 C++语言的“libqrencode”库的实践教程。以. so 共享目标文件(动态库)的形式编译并链接库

# 介绍

**QR 码**是一种机器可读的光学标签，可以包含有关其所附物品的信息。在实践中，二维码通常包含定位器、标识符或跟踪器的数据，这些数据指向…