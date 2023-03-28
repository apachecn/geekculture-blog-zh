# 您的基本 XGBoost 分类代码

> 原文：<https://medium.com/geekculture/your-basic-xgboost-classification-code-38930782c437?source=collection_archive---------9----------------------->

这篇文章是您 XGBoost 之旅的起点

![](img/c87fc4b327ae19d61dee4ff2b712a888.png)

Photo by [Bench Accounting](https://unsplash.com/@benchaccounting?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/online-classes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**XGBoost** 是一个优化的开源软件库，在梯度推进框架下实现优化的分布式梯度推进机器学习算法。XGBoost 由陈天琦创建，最初由分布式(深度)机器学习社区(DMLC)小组维护。它是竞赛中应用机器学习最常用的算法，并通过在结构化和表格数据中赢得解决方案而广受欢迎。

安装 XGB

> pip 安装 xgboost

导入包和库

```
# import statements
import numpy as np *# linear algebra*
import pandas as pd *# data processing, CSV file I/O (e.g. pd.read_csv)*
import xgboost as xgb
from sklearn.cross_validation import train_test_split*#load Dataset*
data = pd.read_csv('../input/diabetes.csv')
data.head()
```

现在我们分割数据集，创建我们的训练集和测试集！

```
*#Split the dataset into train and Test*
seed = 7
test_size = 0.3
X_trian, X_test, y_train, y_test = train_test_split(X_data, y, test_size=test_size, random_state=seed)
```

训练模型

```
*#Train the XGboost Model for Classification*
model1 = xgb.XGBClassifier()
model2 = xgb.XGBClassifier(n_estimators=100, max_depth=8, learning_rate=0.1, subsample=0.5)

train_model1 = model1.fit(X_trian, y_train)
train_model2 = model2.fit(X_trian, y_train)
```

打印分类报告

```
*#prediction and Classification Report*
from sklearn.metrics import classification_report

pred1 = train_model1.predict(X_test)
pred2 = train_model2.predict(X_test)

print('Model 1 XGboost Report **%r**' % (classification_report(y_test, pred1)))
print('Model 2 XGboost Report **%r**' % (classification_report(y_test, pred2)))
```

现在，既然我们已经完成了基础工作，让我们来看看超参数调整

```
*#Let's do a little Gridsearch, Hyperparameter Tunning*
model3 = xgb.XGBClassifier(
 learning_rate =0.1,
 n_estimators=1000,
 max_depth=5,
 min_child_weight=1,
 gamma=0,
 subsample=0.8,
 colsample_bytree=0.8,
 objective= 'binary:logistic',
 nthread=4,
 scale_pos_weight=1,
 seed=27)
```

培训新模型

```
train_model3 = model3.fit(X_trian, y_train)
pred3 = train_model3.predict(X_test)
print("Classification Reportfor model 3: **%.2f**" % (classification_report(y_test, pred3) * 100))
```

这段代码应该作为一个很好的起点！如果你卡住了，一定要伸出手来评论！

其他可能感兴趣的文章:
-[Apache Spark 入门— I | by Sam |极客文化| 2022 年 1 月| Medium](/geekculture/getting-started-with-apache-spark-i-5fbbe7b47667)
-[Apache Spark 入门 II | by Sam |极客文化| 2022 年 1 月| Medium](/geekculture/getting-started-with-apache-spark-ii-fffeab9f5df7)
-[Apache Spark 入门 III | by Sam |极客文化| 2022 年 1 月| Medium](/geekculture/getting-started-with-apache-spark-iii-1758581d87f3)
-[Streamlit 和帕尔默企鹅。上周在网飞的 Binged 非典型… |作者 Sam | Geek Culture | Medium](/geekculture/streamlit-and-palmer-penguins-92a09004ed45)
-[Streamlit 入门。使用 Streamlit 解释你的 EDA 和… | by Sam | Geek Culture | Medium](/geekculture/getting-started-with-streamlit-ed81eafcb298)

干杯，请关注更多此类内容！:)

如果你喜欢它的内容，你现在也可以给我买一杯咖啡！
[samunderscore12 正在创作数据科学内容！(buymeacoffee.com)](https://www.buymeacoffee.com/samunderscore12)