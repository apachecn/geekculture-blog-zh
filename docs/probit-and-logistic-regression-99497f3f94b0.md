# 概率单位和逻辑回归

> 原文：<https://medium.com/geekculture/probit-and-logistic-regression-99497f3f94b0?source=collection_archive---------6----------------------->

当我们需要在 R 中建立一个二进制模型时，我们通常会使用 glm 函数。在二项式部分，我们可以选择两种模型。

```
glm(admit ~ gre + gpa + rank, family = binomial(link = "probit"), data = mydata)glm(admit ~ gre + gpa + rank, family = binomial(link = "logit"), data = mydata)
```

与文献含义相同，二项式(link = "probit ")用于概率单位回归，二项式(link = "logit ")用于逻辑回归。他们都致力于二进制分类。你甚至可以看到很多文章描述他们是…