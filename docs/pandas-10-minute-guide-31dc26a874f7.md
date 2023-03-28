# 熊猫 10 分钟指南

> 原文：<https://medium.com/geekculture/pandas-10-minute-guide-31dc26a874f7?source=collection_archive---------28----------------------->

这将作为开始接触熊猫的基本指南

![](img/c87fc4b327ae19d61dee4ff2b712a888.png)

Photo by [Bench Accounting](https://unsplash.com/@benchaccounting?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/online-classes?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 这是什么？

[pandas](http://pandas.pydata.org/) 是一个开源的 [Python](http://www.python.org/) 库，用于数据分析。Python 在准备和管理数据方面一直很棒，但在分析方面一直不太好——你通常最终会使用 [R](http://www.r-project.org/) 或将其加载到数据库中并使用 SQL(或者更糟，Excel)。Pandas 使 Python 非常适合分析。

```
**import** pandas **as** pd
```

# 载入数据

任何 ML 问题的第一步都是识别你的数据是什么格式，然后将它加载到你正在使用的任何框架中。对于 Kaggle 竞赛，可以在 CSV 文件中找到许多数据，所以这就是我们要使用的示例。

```
df **=** pd**.**read_csv('input.csv')
```

# 基础知识

现在我们在变量 df 中有了数据帧，让我们看看它包含了什么。我们可以使用函数 **head()** 来查看数据帧的前几行(或者使用函数 **tail()** 来查看最后几行)。

```
df**.**head()
df.tail()
```

我们可以使用**形状**属性来查看数据帧的尺寸

```
df**.**shape
```

为了更好地了解我们正在处理的数据类型，我们可以调用 **describe()** 函数来查看关于数据集每一列的统计信息，如平均值、最小值等。

```
df**.**describe()
```

您可以对数据帧中的某些列调用的最有用的函数之一是 **value_counts()** 函数。它显示每个项目在列中出现的次数。

```
df['Column']**.**value_counts()
```

为了让所有列都使用

```
df.columns
```

# 整理

假设我们希望对数据帧中某一列的值进行升序排序

```
df**.**sort_values('Column')
```

# 数据帧迭代

为了遍历数据帧，我们可以使用 **iterrows()** 函数。下面是前两行的示例。iterrows 中的每一行都是一个系列对象

```
**for** index, row **in** df**.**iterrows():
    print row
    **if** index **==** 1:
        **break**
```

# 数据清理

做好的一项重要工作是数据清理。很多时候，您拥有的数据在数据集中会有很多缺失值，您必须识别这些缺失值。下面的 **isnull** 函数将计算出数据帧中是否有任何缺失值，然后对每一列的总数求和。

```
df**.**isnull()**.**sum()
```

# 其他有用的功能

*   drop **()** —这个函数删除您传入的列或行(您也可以指定轴)。
*   agg**()**—aggregate 函数允许您计算每个组的汇总统计信息
*   应用 **()** —允许您将特定函数应用于数据帧或系列中的任何/所有元素
*   get_dummies **()** —有助于将分类数据转化为一个热点向量。
*   drop_duplicates **()** —允许您删除相同的行

这应该是你熊猫之旅的一个非常基本的开始！干杯！
更多资源:

*   [pydata](http://pandas.pydata.org/pandas-docs/stable/10min.html) 教程
*   [数据营](https://www.datacamp.com/community/tutorials/pandas-tutorial-dataframe-python)

如果你卡住了，一定要伸出手来评论！

其他可能感兴趣的文章:
-[Apache Spark 入门—I | by Sam | Geek Culture | Jan 2022 | Medium](/geekculture/getting-started-with-apache-spark-i-5fbbe7b47667)
-[Apache Spark 入门 II | by Sam | Geek Culture | Jan 2022 | Medium](/geekculture/getting-started-with-apache-spark-ii-fffeab9f5df7)
-[Apache Spark 入门 III | by Sam | Geek Culture | Jan 2022 | Medium](/geekculture/getting-started-with-apache-spark-iii-1758581d87f3)
-[Streamlit 和帕尔默企鹅。上周在网飞的 Binged 非典型… |作者 Sam | Geek Culture | Medium](/geekculture/streamlit-and-palmer-penguins-92a09004ed45)
-[Streamlit 入门。使用 Streamlit 解释你的 EDA 和… |作者:Sam | Geek Culture | Medium](/geekculture/getting-started-with-streamlit-ed81eafcb298)

干杯，请关注更多此类内容！:)

如果你喜欢它的内容，你现在也可以给我买一杯咖啡！
[samunderscore12 正在创作数据科学内容！(buymeacoffee.com)](https://www.buymeacoffee.com/samunderscore12)