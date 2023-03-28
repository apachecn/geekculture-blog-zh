# 更新的云 Firestore 测试指南

> 原文：<https://medium.com/geekculture/updated-cloud-firestore-testing-guide-ce146f2b312?source=collection_archive---------19----------------------->

自从我发表了我的[初始测试指南](/@danahartweg/testing-guide-for-cloud-firestore-functions-and-security-rules-39d9f3c92d99)以来，Firestore(特别是本地模拟器)已经走过了*漫长的*之路。在这个更新的指南中，我们将讨论一些对本地仿真器更有帮助的变化，并逐步更新相同的库到最新的包和技术。

为了便于理解，这里是[库](https://github.com/danahartweg/testing-cloud-firestore)，这里是[一步一步的 PR](https://github.com/danahartweg/testing-cloud-firestore/pull/19) 你会在下面的步骤中看到链接。

![](img/ef7b341e33e914369274c7b45aa3f2de.png)

Photo by [Volodymyr Hryshchenko](https://unsplash.com/@lunarts?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)