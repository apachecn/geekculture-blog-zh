# 为什么在你的“和”从句中条件的顺序是至关重要的

> 原文：<https://medium.com/geekculture/why-order-of-conditions-in-your-and-clause-is-a-crucial-one-7e8eeb91ca9?source=collection_archive---------17----------------------->

![](img/851c8b7163551e83c1538c6dd17eac9f.png)

Photo by [Sunder Muthukumaran](https://unsplash.com/@sunder_2k25?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 更好地理解您的数据

这只是我在玩 **Olympics** 数据集时观察到的结果，因为这是一个相对较小的数据集(大约 20 万条记录)，所以差异很小，但当相同的概念应用于更大的数据集时，我相信这会产生巨大的差异。

所以，我想获得在**第二次世界大战(1945)** 后获得奥运会奖牌的德国男性的名单，有两个查询可以获得我们想要的结果，

![](img/b62e548a4b0fb19d406f4cd48a129e17.png)

1st Query

![](img/2230cbcba0eb1dd2b084760ddfcb8d8c.png)

2nd Query

这两个查询给出了相同的结果，但是在分析查询运行时，我们可以看到明显的差异。

![](img/e6b1e4d2db04d97c7124eb39bdb01bc1.png)

Query runtime for the 1st Query

![](img/70ece5edeb04c591bca44a9704e17d99.png)

Query runtime for the 2nd Query

第一个查询的运行时间几乎是第二个查询的两倍。

> **那么为什么会发生这种情况呢？？？？**

因此，让我们计算一下满足 AND 子句中每个条件的记录总数。

a)1945 年以上的记录数量

![](img/b9d96809f61d8da9e53245b2cac06bd9.png)

Returning count of records having value of the column Year>1945

![](img/5eb5acbf005306706842c3898b9ac283.png)

There are total of 232344 Records that have value of the column Year>1945

b)发生性行为的记录数量='M '

![](img/8e6ba15157c7041b41978c0585c1cc88.png)

Returning count of records having value of the column Sex=’M’

![](img/68bfd10d9c77223baf72d040980c87f2.png)

There are total of 196594 Records that have value of the column Sex=’M’

c)奖牌栏中具有非空值的记录的数量

![](img/386e4e868d69086968cb950b25b0e0dd.png)

Returning count of records having non null values in the column Medal

![](img/ce75b8c8e17053cb8e161620c11e6897.png)

There are total of 39783 records that have non null values in the Medal column

d)团队=“德国”的记录数量

![](img/71df2926391dcfa188d4b21b920eab1f.png)

Returning count of records that have the value of the column Team = ‘Germany’

![](img/0744888e39c18ea44e0716dc3661da1f.png)

There are a total of 9326 records that have the value Germany in the team column

从上面的结果中，您可以大致了解为什么第二个查询的运行时间较短，这是因为在第一个查询中，第一个条件总共满足 2，32，344 条记录，然后检查第二个条件，依此类推，而在第二个查询中，第一个条件仅满足 9，326 条记录，然后检查第二个条件，依此类推…

让我们更深入地研究一下，找到满足两个查询的每个步骤的记录数

> **第一次查询:**
> 
> 1)年份> 1945 且性别='M '的记录数

![](img/e8dccb1483f2b2bebfc06a76a5477684.png)

Returning count of records having year>1945 and value of Sex column = ‘M’

![](img/8fbc65ba9a5f5effa26154f98a581f3a.png)

There are total of 159766 records that satisfies both of this condition

> 2)年份> 1945 年、性别='M '且奖牌栏中的值不为空的记录数

![](img/fd44c99e68e433b99c9cfcc198ab0724.png)

Returning count of records having year>1945 and value of Sex column = ‘M’ and non null values in Medal column

![](img/77004d024821aa9601829c961726d9c2.png)

There are total of 20649 records that satisfies these 3 condition

> 3)年份> 1945 年、性别='M '以及奖牌列和团队= '德国'中的非空值的记录数

![](img/a20e1fc65c3d909276252f03f9ae6770.png)

Returning count of records having year>1945 and value of Sex column = ‘M’ and non null values in Medal column and value of Team column = ‘Germany’

![](img/a69c2ae434454ceaab9cb0f4ccabb0a2.png)

There are total of 887 records that satisfies the 4 conditions

***因此，第一个查询将首先处理 2，32，344 条记录，然后处理 1，59，766 条记录，然后处理 20，649 条记录，最后处理 887 条记录，然后从这 887 条记录中返回运动员的不同姓名。***

现在，我们将对第二个查询进行同样的检查

> 第二次查询:
> 
> 1)在奖牌栏中具有团队=“德国”和非空值的记录的数量

![](img/07b119abd3b9f2d0a21c31a6efe0e9f9.png)

Returning the count of records that have value of column team as Germany and non null values in Medal columns

![](img/ab6cc0157cc8b260f8aba1bac4c44e05.png)

There are total of 1984 records that satisfies both of the above condition

> 2)在奖牌列中具有团队= '德国'和非空值以及性别列的值='M '的记录数

![](img/ebc81167d5d41942f3b33e437ca5fd8e.png)

Returning the count of records that have value of column team as Germany and non null values in Medal columns and value of the column sex=’M’

![](img/6185317dab6a6c468996d8f0e9920664.png)

There are total of 1327 records that satisfies the above conditions

> 3)在奖牌列中具有团队= '德国'和非空值以及性别列的值='M '和年份> 1945 的记录数

![](img/c360da3bccf8b2f400907252305b5755.png)

Returning the count of records that have value of column team as Germany and non null values in Medal columns and value of the column sex=’M’ and Year>1945

![](img/d53b6698d50c2931222afcb0d298e31a.png)

There are total of 887 records that satisfies the above condition

***因此，第二个查询将首先处理 9326 条记录，然后处理 1984 条记录，再处理 1327 条记录，然后处理 887 条记录并找到运动员的不同姓名。***

这就是为什么第二个查询比第一个查询只花了一半的时间。

## 总结:

最后要说明的是，这是一个人对数据的理解至关重要的原因之一，因为看起来很小的变化会在查询运行时产生巨大的变化，所以当相同的 and 条件放在更复杂的查询中时，会在运行时产生很大的变化。

如果你觉得这篇文章很有帮助，请拍下这篇文章，如果你对这篇文章有什么建议，可以发表评论。

> 谢谢你