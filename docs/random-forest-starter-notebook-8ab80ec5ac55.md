# 随机森林入门笔记本！

> 原文：<https://medium.com/geekculture/random-forest-starter-notebook-8ab80ec5ac55?source=collection_archive---------11----------------------->

您可以为随机森林用例修改的起始代码

![](img/c87fc4b327ae19d61dee4ff2b712a888.png)

Photo by [Bench Accounting](https://unsplash.com/@benchaccounting?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/online-classes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

andom forest 是一种监督学习算法，用于分类和回归。有许多不同的模型可用于对分类数据进行预测。逻辑回归是最常见的二项式数据之一。其他方法包括支持向量机(“SVMs”)、[朴素贝叶斯](https://en.wikipedia.org/wiki/Naive_Bayes_classifier)和 [k 近邻](https://en.wikipedia.org/wiki/K-nearest_neighbors_algorithm)。在一个模型有大量特征的场景中，随机森林往往会大放异彩，这些特征各自的预测能力较弱，但集合起来的预测能力要强得多。

在本文中，我将向您快速介绍如何用 Python 实现随机森林模型来解决分类问题。

导入库:

```
*# Imports*

*# pandas*
import pandas as pd
from pandas import Series,DataFrame

*# numpy, matplotlib, seaborn*
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
sns.set_style('whitegrid')
%matplotlib inline

*# machine learning*
from sklearn.linear_model import LogisticRegression
from sklearn.svm import SVC, LinearSVC
from sklearn.ensemble import RandomForestClassifier,GradientBoostingClassifier
from sklearn.neighbors import KNeighborsClassifier
from sklearn.naive_bayes import GaussianNB
```

导入数据:

```
*# get titanic & test csv files as a DataFrame*
titanic_df = pd.read_csv("../input/train.csv")
test_df    = pd.read_csv("../input/test.csv")
```

丢弃不必要的数据:

```
*# drop unnecessary columns, these columns won't be useful in analysis and prediction*
titanic_df = titanic_df.drop(['PassengerId','Name','Ticket'], axis=1)
test_df    = test_df.drop(['Name','Ticket'], axis=1)
```

定义培训和测试数据分割:

```
*# define training and testing sets*

X_train = titanic_df.drop("Survived",axis=1)
Y_train = titanic_df["Survived"]
X_test  = test_df.drop("PassengerId",axis=1).copy()
```

训练模型:

```
*# Random Forests*

random_forest = RandomForestClassifier(n_estimators=100,oob_score=True,max_features=5)
random_forest.fit(X_train, Y_train)
Y_pred = random_forest.predict(X_test)
random_forest.score(X_train, Y_train)OP : 0.9640852974186308
```

这是一个关于随机森林入门的简单教程！

如果你卡住了，一定要伸出手来评论！

其他可能感兴趣的文章:

*   熊猫 10 分钟指南。这将作为一个基本的指南来获得… |由山姆|极客文化| 2022 年 1 月|媒体
*   [与 Seaborn 的美好剧情。创造情节让你… |作者:山姆|极客文化| 2022 年 1 月| Medium](/geekculture/beautiful-plots-with-seaborn-100f0ce7a285)
*   [您的 go to Numpy 清单。快速浏览所有重要的… |作者 Sam |极客文化| 2022 年 1 月| Medium](/geekculture/your-go-to-numpy-checklist-96ec1e7cd1c3)
*   [您的 go to Numpy 清单。快速浏览所有重要的… |作者 Sam |极客文化| 2022 年 1 月| Medium](/geekculture/your-go-to-numpy-checklist-96ec1e7cd1c3)
*   [Apache Spark-I 入门|作者 Sam |极客文化| 2022 年 1 月| Medium](/geekculture/getting-started-with-apache-spark-i-5fbbe7b47667)

干杯，请关注更多此类内容！:)