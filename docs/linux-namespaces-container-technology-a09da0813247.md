# Linux 名称空间(容器技术)

> 原文：<https://medium.com/geekculture/linux-namespaces-container-technology-a09da0813247?source=collection_archive---------4----------------------->

Linux 上的容器利用了 Linux 内核提供的名称空间。在 Linux 上，容器作为普通进程运行，与其他进程共享同一个内核。在 Windows 或 Mac 上，首先提供一个虚拟机。所有容器都在虚拟机内部运行。

# **资源名称空间**

在计算机科学中，一切都可以用一个对象来表示，包括网络设备、进程、线程、PID 号、路由表、文件系统等。