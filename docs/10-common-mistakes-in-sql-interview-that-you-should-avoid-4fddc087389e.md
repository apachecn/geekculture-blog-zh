# 你应该避免的 SQL 面试中的 10 个常见错误

> 原文：<https://medium.com/geekculture/10-common-mistakes-in-sql-interview-that-you-should-avoid-4fddc087389e?source=collection_archive---------15----------------------->

![](img/e2f7e3f6782c10b3faf172cae571d90a.png)

Photo by [John Schnobrich](https://unsplash.com/@johnschno?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

经过多次面试，我刚刚找到了一份数据科学家的工作。几乎所有这些采访都有 SQL 现场编码会议。根据我对 SQL leetcode 问题的实践和我的 SQL 面试，我总结了数据科学 SQL 面试中最常见的 10 个错误。其中一些可能是小错误，但可能会降低你获得理想工作的机会。

所以让我们开始吧。

![](img/5e994adbba333c72468c4f459dfc38c9.png)

Photo by [Stephen Leonardi](https://unsplash.com/@stephenleo1982?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 1.没仔细看问题

如果我答错了，十有八九的情况是*我没有认真看问题。*在面试中，有些人想节省时间，所以他们会略读问题。

不要！

你需要花多少时间来理解这个问题。如果你有任何问题，不要犹豫问面试官！

有些问题可能被设计得含糊不清，导致误解。不要害怕问面试官。沟通是面试过程中非常重要的一部分！！

## 2.忘记筛选 WHERE 语句中的条件

我经常犯的第二个最常见的错误是忘记过滤 WHERE 语句中的条件。

它很容易被遗忘，尤其是如果您刚刚编写了一个超长查询，其中包含所有那些复杂的窗口函数和子查询。

所以如果问题需要你过滤“2022 年 6 月美国移动 ios 用户”(例如)，一定要用 WHERE 过滤最终答案。

## 3.忘记订购了

使用 ORDER BY 有两件事值得注意。

首先，人们可能会忘记在语句的末尾写上 ORDER BY。特别是答案包含日期/月份/年份，需要按日期列排序。总是检查是否需要按列排序是一个好习惯，即使您没有被要求这样做。

第二件事是检查是按 DESC(降序)还是 ASC(升序)排序。如果没有指定，默认顺序是 ASC。

## 4.忘记四舍五入了

如果问题是关于一个平均数字(比如平均参与度、平均月收入……)，总是问面试官你是否需要将答案四舍五入到 2 位数。

如果你没有那样做，你不会被扣分。但是如果你问了，把数字四舍五入，说明你在意小数点最后的格式。

## 5.没有乘以 100 如果答案是一个百分比

这是一个我经常滑入的陷阱。如果问题是让你计算一个百分比(比如参与率)，你很容易忘记最后要乘以 100。所以要格外小心百分比！

## 6.WHEN 语句中没有放入 END

情况
当条件 1 然后结果 1
当条件 2 然后结果 2
当条件 n 然后结果 N
否则结果
结束；

别忘了**结尾**！

## 7.忘记给新列命名了

大多数人无法给经历了完整计算的新列命名。同样，你可能不会丢分。但是在真正的专业数据科学家工作中，需要将新列**写成新名称**存储到数据库中。

新名称也有助于过滤器。因此，命名您计算的每个新列！

## 8.当可以使用 GROUP BY 和 HAVING 时，总是编写子查询

子查询仍然可以得到答案，但有时该问题旨在测试您在 GROUP BY、HAVING 或 window 函数方面的知识。

因此，在编写一个长的嵌套子查询之前，首先问问自己是否能以更优化的方式编写查询，比如使用 GROUP BY、HAVING 和 window 函数。

## 9.日期格式/函数有误

日期格式可能比较棘手，所以我建议您花时间编写关于日期格式/函数的文档。

取决于你是用 MySQL 还是 PostgreSQL 面试，有很多不同之处。

问问你自己，你是否能够:

*   从日期中提取日/周/月
*   计算两个日期之间的时差
*   将日期转换成特定的格式

还有更多

## 10.最后没有仔细检查

SQL 面试中最大的错误是没有在最后仔细检查你的答案。

因为人确实会犯错。

写完查询后，您就完成了一半。不要急着说“我完了！”直到你检查完你的每一行代码。

当你检查代码时，你也可以大声对面试官说出你的想法，让他们的生活更轻松。

在面试官通知你之前，这个错误不叫错误。所以要经常复查！

![](img/000b75393199a8f3a0e225a2964533dc.png)

Photo by [Agence Olloweb](https://unsplash.com/@olloweb?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我计划写一系列 SQL、编码和数据科学家面试技巧(所以别忘了关注，这样你就不会错过它们了)。

如果你喜欢这篇文章或者觉得有用，可以点击拍手键告诉我:)如果你还有其他问题或者建议的话题，请留下评论(我都看了)。

祝你的 SQL 和数据科学面试好运！