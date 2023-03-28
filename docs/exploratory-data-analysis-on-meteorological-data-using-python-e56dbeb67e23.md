# 基于 Python 的气象数据探索性分析

> 原文：<https://medium.com/geekculture/exploratory-data-analysis-on-meteorological-data-using-python-e56dbeb67e23?source=collection_archive---------3----------------------->

探索性数据分析是一种分析数据、总结数据主要特征和更好地理解数据集的方法。它还允许我们快速解释数据，并调整不同的变量以查看它们的效果。获得一个完美的 EDA 的三个主要步骤是从一个授权的源提取数据、清洗和处理数据、以及对清洗后的数据集执行数据可视化。

一种更容易在网上找到的数据是天气数据。许多站点提供许多气象参数的历史数据，如压力、温度、湿度、风速、能见度等。

我们将分析天气数据集。你可以从提到的链接([https://www.kaggle.com/muthuj7/weather-dataset](https://www.kaggle.com/muthuj7/weather-dataset))中找到数据集。我们的主要目标是执行数据清理、数据标准化、测试假设，并得出适当的见解。让我们开始吧！

**1。导入必要的库**

![](img/604355a130f4d6a7b07852c8b63145f2.png)

Libraries imported — Pandas, NumPy, Matplotlib, Seaborn

**2。读取数据集**

![](img/7cbc92222bce03930c509d92cf692ee6.png)

Reading the dataset

这里我显示了数据集的前五行:

![](img/bccc90ae5c4d7514379bb979a1ab88c0.png)

**3。清理和处理数据集**

我们首先将所有数值数据从 object 数据类型转换为 float/int 数据类型:

![](img/e1177ca83d0626c7e87e23125f589354.png)

然后我们，删除所有的空值:

![](img/6c8c7ca710ca8ee9aefc5bf92ccce8f8.png)

以下是代表相同情况的热图:

![](img/908a7d23bb4d7db05b0dc4cbaaeebb77.png)

我们还将日期转换成标准的日期时间格式:

![](img/4c62dbbeaf4482d7e9aa92a6a6aaff6b.png)![](img/ee28f588a89c886b64de6e59d1b0477e.png)

in Datetime format

**4。执行探索性数据分析**

a.从“摘要”栏中分析最频繁的天气

![](img/5b63ff82942d7ab63d8de3b851a5466e.png)

Most Frequent Weather Type

代表上述频率的线图如下所示。我们可以观察到最常见的天气是“部分多云”

![](img/47d1aef62ef99fcb35d91ce4a26ffd38.png)

b.天气温度；天气湿度；天气 v/s 压力

![](img/3ebc3168c1c4b2779fe323339d4faa78.png)

Weather V/S Temperature

从上图中我们可以看出，最高温度出现在天气“干燥”的时候。

![](img/a7421834c6ebe3cdb68d497a9f5ae864.png)

Weather V/S Humidity

上图说明了最大湿度适用于各种天气类型— *多雾、* *微风、多雾和下雨。*

![](img/db159d0388ebc5b1a8cd784368a5d562.png)

Weather V/S Pressure

**5。仪表板**

仪表板是一种图形用户界面，通常提供与特定目标或业务流程相关的关键绩效指标的概览视图。在其他用法中，“仪表板”是“进度报告”或“报表”的另一个名称，被认为是数据可视化的一种形式。

a.安装必要的软件包

![](img/445a2860ab772ae4c62c8d66b053150a.png)

Installing Pywedge Package

b.导入包

![](img/9fb3637f4e6318f8ce01102659bb19c7.png)

c.创建仪表板

![](img/9d47888066e23b6dc831e69ff85ca4d4.png)

d.散点图和饼图

![](img/4894dd9e977eac4070cf37567e7da646.png)

Scatter Plot Dashboard

![](img/ee95e373e87da9c8c52481027a4c3959.png)

Pie Chart Dashboard

e.条形图

![](img/88e7d0ce90c517634d50dd25a218cffc.png)

Bar Plot Dashboard

f.相关图

![](img/d029791e875d657748f2768dc37c041c.png)

Correlation Plot

使用仪表板可以创建许多其他图，如*小提琴图、箱线图、分布图和直方图。*

**6。结论**

全球变暖增加了某些极端天气的频率和强度。通过分析，我们收集了过去 10 年的有用见解。例如，变暖导致暴雨中的降雨量增加。两次降雨之间的干旱期也更长。这一点，加上更高的温度导致更多的蒸发。潮湿的地方通常变得更加潮湿，而干燥的地方变得更加干燥。