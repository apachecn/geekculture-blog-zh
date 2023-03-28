# 使用 Plotly Express 创建和嵌入交互式散点图

> 原文：<https://medium.com/geekculture/using-plotly-express-to-create-interactive-graphs-of-the-greatest-scorers-in-nba-playoff-history-3f0cc085d933?source=collection_archive---------36----------------------->

使用 Plotly Express、Python 和 Pandas 一步步绘制 NBA 历史上领先的季后赛得分王。

![](img/b867a2a675ed749f482ead6c2788d6eb.png)

在处理数据时，创建视觉上吸引人的图表是传达信息的重要部分。具有易于区分的点/线、有用的图例以及缩放和悬停(仅举两个例子)等交互功能的图形比没有这些的图形更具信息量和用户友好性。

虽然有一些方法可以做到这一点，但我发现了一个我觉得非常简单而强大的方法: [Plotly Express](https://plotly.com/python/plotly-express/) 。**在这篇文章中，我将使用 Plotly、Python 和 Pandas 创建(并嵌入)NBA 历史上领先的季后赛得分者的交互式图表。**

让我们看几个简单的例子。

## 步骤 1:检索数据

在本文中，我不打算展示我用来调用 API 和创建 DataFrame 的代码，**但是我强烈建议您** [**访问我的 GitHub 库**](https://github.com/wmblack23/NBA-Playoff-Leading-Scorers-Interactive-Jupyter-ScatterPlot/blob/main/Playoff%20Points%20Leaders.ipynb) **并亲自查看笔记本**。它被注释掉了，很容易理解。

> Y 您也可以[下载 Excel 文件](https://github.com/wmblack23/NBA-Playoff-Leading-Scorers-Interactive-Jupyter-ScatterPlot/raw/main/Playoff%20DataFrame.xlsx)，其中包含我在本文中使用的所有 NBA 季后赛数据，然后按照提供的代码一步一步地操作。

## 步骤 2:加载和组织数据

导入我们的库:

将我们的 Excel 文件加载到熊猫数据框架中:

Creating our DataFrame and renaming column ‘0’: ‘Player Name’.

Interactive DataTable for all players who have played in the NBA postseason

我还想添加第四列——“季后赛 PPG”(每场比赛的得分):

Adding column ‘Playoff PPG’

DataTable sorted by ‘Playoff Points Scored’

## 步骤 3:绘制图表的时间

使用 Plotly 创建散点图:

Creating a simple Plotly scatter plot

*   **数据帧** —设置为等于我的数据帧:“播放数据帧”
*   **x**—x 轴的值:“已进行的季后赛比赛”。数据框架中的列。
*   **y**—y 轴的值:“季后赛得分”。数据框架中的列。
*   **标题** —为情节设置标题。

[https://datapane.com/u/wmblack23/reports/nba-playoff-scoring-all-players](https://datapane.com/u/wmblack23/reports/nba-playoff-scoring-all-players)

还不错！

将鼠标悬停在图上的任意一点上，我们可以看到该点对应的季后赛点数和比赛数。这是一个有用的特性——但是如果当我们将鼠标悬停在某个点上时，还能看到玩家的名字，那就更有用了。*提示，提示。*

我们还可以在图形的任何点上单击并拖动(创建一个矩形)来放大。这在这样一张有如此多的点聚集在一起的图表上尤其有用。

## **添加视觉特征**

Adding visual features to our scatter plot

*   **大小** —我们指定“季后赛得分”列来告诉我们的图表，使季后赛总得分较高的球员得分较大，而季后赛总得分较低的球员得分较小。
*   **模板** — Plotly 有一些模板选项来设计你的情节(包含在 GitHub repo 中)。我喜欢白色背景。
*   **颜色** —我们为颜色变量指定“季后赛 PPG”。这意味着我们将为具有相似“季后赛 PPG”值的分数赋予相似的颜色。
*   **Size_max** —图上任意点的最大尺寸。
*   **Hover_name** —现在，当我们悬停在一个点上时，我们会看到“玩家姓名”作为第一个值。
*   **Hover_data** —在我们的 Hover_data 中包含“季后赛 PPG”，这样我们就可以看到它和“球员姓名”以及我们的 x 和 y 值。
*   我选择“Jet”作为我的**色标**。[更多在此](https://plotly.com/python/builtin-colorscales/)。

[https://datapane.com/u/wmblack23/reports/nba-playoff-scoring-adding-features](https://datapane.com/u/wmblack23/reports/nba-playoff-scoring-adding-features)

> 通过将颜色设置为“PPG ”,大小设置为“总点数”,我们甚至不需要使用缩放和悬停功能就可以轻松进行观察。

## **前 200 名—附加功能**

Adding statistical features to our scatter plot

*   **趋势线**—‘ols’:普通最小二乘回归线。这允许我们对我们的数据进行进一步的观察。哪些球员的得分高于、低于或达到了他们打了多少场季后赛的预期分数？试着悬停在回归线上！
*   **Add_hline** —添加一条水平线，代表前 200 名得分者的季后赛得分中位数。
*   **Add_vline** —添加一条垂直线，代表前 200 名得分者参加的季后赛比赛的中位数。

[https://datapane.com/u/wmblack23/reports/top200-regression-and-more](https://datapane.com/u/wmblack23/reports/top200-regression-and-more)

两个“add_line”功能，如上面的趋势线，允许我们对玩家进行额外的观察:

*   在右下象限的丹尼斯·罗德曼，比这个名单上的平均球员打了更多的季后赛，但平均只有 6.4 次 PPG，所以他的得分远低于中位数。
*   同样，在左上方，与前 200 名(29.73 PPG)相比，阿伦·艾弗森的得分高于平均水平，但他的比赛次数明显低于平均水平，所以他在比赛中位于中线的左侧。

那些在右上象限的球员，与前 200 名的其他球员相比，以高于平均水平的速度出现和生产的球员，都是目前或未来的顶级 NBA 巨星；**尤其是那些也落在回归线以上的。**

事实上，如果我们想更仔细地看看谁组成了 NBA 季后赛得分王的超级精英群体，我们可以通过以下方式来实现:

The most elite group of playoff performers in NBA history

这些球员**参加了更多的季后赛比赛** ( > 118)和**获得了更多的季后赛分数** ( > 1959)超过了前 200 名季后赛得分王的平均分数**超过了我们对 OLS 趋势线**的预测。

如果你像我在这里做的一样，对学习如何使用[数据面板](https://datapane.com/)嵌入表格和可视化感兴趣，我鼓励你[看看这篇有用的文章](https://towardsdatascience.com/how-to-embed-interactive-charts-on-your-medium-articles-and-website-6987f7b28472)。

用 DataPane 嵌入是一个非常方便且易于使用的特性，有助于读者更容易访问您的数据。

感谢您的阅读！

查看文章顶部的 GitHub 链接了解更多信息。

迈克尔·布莱克