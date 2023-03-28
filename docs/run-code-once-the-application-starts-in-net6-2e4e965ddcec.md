# 一旦应用程序在. Net6 中启动，就运行代码

> 原文：<https://medium.com/geekculture/run-code-once-the-application-starts-in-net6-2e4e965ddcec?source=collection_archive---------1----------------------->

一旦应用程序启动，您可能需要执行一些代码。像日志一样，如果您是开发人员，您可能需要在内存中填充一些数据、运行后台任务、发布事件或读取配置。

## Program.cs 文件

您可能想到的第一件事是在 Program.cs 文件中编写代码。. Net6 确保这些方法以及其中的代码只在应用程序启动时执行。
开发者可以用两种方式运行代码，在构建者之间。服务(…)在这里…