# 如何改变 Git 分支的基础

> 原文：<https://medium.com/geekculture/how-to-rebase-git-branches-d3026cc0d5f0?source=collection_archive---------9----------------------->

![](img/68b993b91393a0ad777bf86840c0c402.png)

Photo by [Praveen Thirumurugan](https://unsplash.com/@praveentcom?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/git?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

假设您正在处理分支 1，而您的同事正在处理分支 2。你现在的工作依赖于他/她的代码，所以你必须把代码取回来。这就是 git rebase 可以为我们做的工作。

> R 基础是将一系列提交移动或合并到一个新的基础提交的过程。

1.  **确保你切换到当前的工作分支**