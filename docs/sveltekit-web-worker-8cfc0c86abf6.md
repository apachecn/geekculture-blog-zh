# SvelteKit Web Worker

> 原文：<https://medium.com/geekculture/sveltekit-web-worker-8cfc0c86abf6?source=collection_archive---------0----------------------->

## 如何在 SvelteKit 中创建网络工作者并与之交流

![](img/d3a48bb091ec2ae088c75d2c0ec754b1.png)

Photo by [Christopher Burns](https://unsplash.com/@christopher__burns?utm_source=Papyrs&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我❤️ [网络工作者](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API/Using_web_workers)！

这些后台线程是我计算任何我认为对前端 web 应用程序来说太昂贵的东西的策略(例如[领带追踪器](https://tietracker.com/))或者如果它必须执行一个循环任务(例如[纸莎草纸](https://papy.rs/)或[周期。观看](https://cycles.watch/))。