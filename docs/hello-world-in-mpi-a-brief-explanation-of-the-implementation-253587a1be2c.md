# MPI 中的 Hello World 实现的简要说明

> 原文：<https://medium.com/geekculture/hello-world-in-mpi-a-brief-explanation-of-the-implementation-253587a1be2c?source=collection_archive---------19----------------------->

在我们的[上一篇文章](https://himnickson.medium.com/configuring-mpi-on-windows-10-and-executing-the-hello-world-program-in-visual-studio-code-2019-879776f6493f)中，我们讨论了在 WIndows 10 机器中设置 MPI，并简单地通过 Hello World 程序验证了 MPI。

![](img/0783777e7731c607e56b90b8842a4e9c.png)

Photo by [Gustas Brazaitis](https://unsplash.com/@gustasbrazaitis?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

让我们直接进入那一课的代码。下面给出了实现的代码。

```
#include "mpi.h"
#include <stdio.h>
int main(int argc, char* argv[])
{
    // Initialize the MPI environment…
```