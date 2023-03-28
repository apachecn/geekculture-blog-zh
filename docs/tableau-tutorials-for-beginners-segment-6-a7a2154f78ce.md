# 初学者 Tableau 教程—第 6 部分

> 原文：<https://medium.com/geekculture/tableau-tutorials-for-beginners-segment-6-a7a2154f78ce?source=collection_archive---------32----------------------->

![](img/973363d74238eacc21d569a6d04e8d00.png)

朋友们好！希望你已经阅读并喜欢本教程的第 5 部分。如果您还没有，请抽出 10 分钟的宝贵时间来理解**第五部分**中解释的概念。

你可以在这个链接([**Tableau 初学者教程—第 5 段**](/geekculture/tableau-tutorials-for-beginners-segment-5-54fdee46399d?source=friends_link&sk=b7de109393b03c0a0fb39ff3c8944497) )获取。

在今天的文章中，我们将了解以下概念:

*   双轴

**双轴:**这是 tableau 中的一个特性，利用它，我们可以为图表创建一个副轴。例如，我们可能希望创建一个图表，其中主 Y 轴代表总销售额，次 Y 轴代表总利润。我们可以使用双轴特性来实现这一点。让我们看看如何。

![](img/3088606c9992380cedc8254147938d9e.png)

在上面的例子中，我在主 Y 轴**上绘制了 **SUM(Sales)** ，在 X 轴上绘制了子类。我现在想在**次 Y 轴**中添加 **SUM(Profit)** 作为一行。**

首先，我将 **SUM(Profit)** 拖放到画布的行中。

![](img/1e89753796f38afc9406acd9426d6a9d.png)

接下来，我通过标记卡将 **SUM(Profit)** 的图表类型更改为线形:

![](img/d2db470d9495bcd3443e4f8feaf61eab.png)

图表现在看起来是这样的:

![](img/a987e0134e0d62c02b5a3af027d6a103.png)

右键单击利润轴，然后单击双轴选项

![](img/7bb4cc278729119893c85079e48f61cb.png)

点击双轴后，图表将形成利润的副轴:

![](img/d3d36f1ec1ce76b026ddeff55cafe041.png)

**注意**:点击双轴后，如果没有出现条形，只需使用标记卡将 **SUM(Sales)** 的图表类型改为条形即可。