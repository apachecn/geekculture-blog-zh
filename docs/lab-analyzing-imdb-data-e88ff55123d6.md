# 实验室:分析 IMDb 数据

> 原文：<https://medium.com/geekculture/lab-analyzing-imdb-data-e88ff55123d6?source=collection_archive---------11----------------------->

在这个实验室里，我完成了一系列探索 IMDb 电影分级数据的练习。我对 IMDB 的电影数据进行了基本的探索性数据分析，试图回答如下问题:每种类型的平均评分是多少？一部电影里有多少不同的演员？

**基础级**

让我们导入必要的库:

![](img/b738da41c874429f4e814941034a2ea1.png)

## 读入“imdb_1000.csv ”,并将其存储在名为“movies”的数据帧中。

![](img/207eda2f7d1bce41d4cc4f93a356ce3b.png)

## 检查行数和列数

![](img/6a5d79c570d41c5f1065bbb953c3fa09.png)

## 检查每列的数据类型。

![](img/6aa68a5e3519d169b714fce4a1f6d6ff.png)

## 计算平均电影时长。

![](img/de241ae996b1e7a09ba0ae4aa969b476.png)

## 按时长对数据帧进行排序，找出最短和最长的电影。

![](img/69fafd69045cce190a1f902901d2aeda.png)

## 创建一个时间长度直方图，选择一个“适当”的媒体夹数量。

![](img/6acd3188cbf2d3c4e2433487de659681.png)

## 使用盒状图显示相同数据。

![](img/1e5392b211ec603b5346091c77d705f8.png)![](img/cad9ac5629ed7c93ebed5df5570161a5.png)

# 中间能级

## 计算有多少部电影具有每种内容分级。

![](img/3b76ce600924d42e0bab9c26c1078893.png)

## 使用可视化来显示相同的数据，包括一个标题以及 x 和 y 标签。

![](img/8faad6ceb6e99c9b5974c2e5416f8e15.png)

## 将以下内容分级转换为“未分级”:未分级、批准、通过、一般。

![](img/e8c8b2a8148cd85c401117ab0f6e684e.png)

## 将以下内容分级转换为“NC-17”:X，TV-MA。

![](img/8436ab905eb40b9e3f2b9b26b36f5985.png)![](img/d72a4dbc888ee5826f1591b66048b394.png)![](img/f45e9e6c4d5716a44e2b3dc6a1cb195e.png)

## 计算每列中缺失值的数量。

![](img/a7c8f6fc73e7349c12559b8ba95a39a8.png)

## 如果有缺少的值:检查它们，然后用“合理的”值填充它们。

![](img/321d81e0ce226ebc8d8474da5db16aec.png)

## 计算 2 小时或更长时间的电影的平均星级，并与短于 2 小时的电影的平均星级进行比较。

![](img/59aa9bb9a19c2d8813cf149a95a16ffc.png)

## 用可视化来检测时长和星级是否有关系。

![](img/ed201e1510c16167e46c8ee6378b8bb4.png)![](img/eac71d7964d82c42d576a8b8d6d53812.png)

## 计算每个流派的平均持续时间。

![](img/09ff87d43d60d5ebd6d4ccff5c28ba7b.png)

# 优等

![](img/9dbbb614021d9362068a381ef5c94ec4.png)

## 找到每种类型中星级最高的电影的标题

![](img/dfbc16fbbeaee9b4792be9b8d2f85379.png)

## 检查是否有多个同名电影，如果有，确定它们是否实际上是重复的。

![](img/066457292bfbf112840f082444678637.png)

## 计算每种类型的平均星级，但只包括至少有 10 部电影的类型

![](img/d2439b899acd3b587c886f084d7dfb80.png)