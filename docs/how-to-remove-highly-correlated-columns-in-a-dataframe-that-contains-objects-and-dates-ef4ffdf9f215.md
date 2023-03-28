# 如何删除包含对象和日期的数据帧中高度相关的列

> 原文：<https://medium.com/geekculture/how-to-remove-highly-correlated-columns-in-a-dataframe-that-contains-objects-and-dates-ef4ffdf9f215?source=collection_archive---------10----------------------->

![](img/6c5c20fe29e6268f9d7f003820a993ff.png)

每当处理大型数据集时，您可能希望减少该数据集中的列数，以减少数据集中的冗余和噪音量。如果数据集中的所有列都是数值型的，那么减少数据集中的列是一件相对容易的事情，但是，如果它们包含字符串或日期，那么就可能会更…