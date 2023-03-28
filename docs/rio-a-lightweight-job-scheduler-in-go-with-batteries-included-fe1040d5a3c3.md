# Rio——内置电池的轻量级 Go 作业调度器

> 原文：<https://medium.com/geekculture/rio-a-lightweight-job-scheduler-in-go-with-batteries-included-fe1040d5a3c3?source=collection_archive---------16----------------------->

![](img/c66c539c0900c9ac5403289aff79d04a.png)

Photo by [Joey Kyber](https://unsplash.com/@jtkyber1?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/timelapse?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 里约是什么？

Rio 是一个轻量级作业调度器和作业链库。它主要是为 Golang web 应用程序构建的，但它可以非常容易地为任何需要作业调度的应用程序服务。该库是一个异步作业处理器，它通过**重试**、**超时**和**上下文**、**取消**功能异步地进行所有的后台调用。