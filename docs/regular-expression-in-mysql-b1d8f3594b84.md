# MYSQL 中的正则表达式

> 原文：<https://medium.com/geekculture/regular-expression-in-mysql-b1d8f3594b84?source=collection_archive---------16----------------------->

![](img/90d30ab1019e9e1b690e3f35ec8177b8.png)

Image from [Shutterstock](https://www.shutterstock.com/image-illustration/conceptual-business-illustration-words-regular-expression-1012948930)

## 使用这些查询改进您的模式匹配

正则表达式:

正则表达式是用于定义搜索模式的字符序列/模式，主要用于 C++和 python 等编程语言中的字符串搜索算法和模式匹配。

模式匹配也广泛用于 SQL 查询中，以查找以特定模式出现的数据。

本文试图解释 MySQL 中的所有正则表达式，并在 Employees 数据库和 Olympics 数据库中执行相同的操作。

![](img/78d8648e564522646a8bfc5742f92508.png)

Snippet from the Olympics Database

![](img/8184c3412b6a2803ed2069bd197140a6.png)

Snippet from the Employee Database

> ^ :

匹配字符串的开头。

![](img/589899a640e9a03a73db7ee3585bd868.png)

Query to obtain all the records that has last name starting with Al

![](img/aeeaf587d2a0561149ee44d865e34414.png)

Answer for the above query

需要注意的重要一点是 MySQL 中的 regexp 不区分大小写，所以下面的查询给出的输出与上面的一样。

![](img/8acb248113c2233cb657af587acb7ca2.png)

Here instead of Al, al is used but since regexp in MySQL is not case sensitive this query gives the same output as the last one

![](img/aeeaf587d2a0561149ee44d865e34414.png)

Answer for the above query

> $ :

匹配字符串的结尾。

![](img/0bf23790c823aca85782b59c55feacc5.png)

query to return records with last_name ending with ar

![](img/cd4c7528a392f79ef90a87790e0d59a9.png)

answer for the above query

> 。或者。{} :

点字符匹配除新行之外的任何单个字符，可用于定义您希望一个字符串包含的字符数，

![](img/9e50da1aada20b5b45e2b002099a34c2.png)

query to obtain last_name having 5 characters

此查询使用^和$匹配姓氏的开头和结尾以及五个'.'定义 5 个字符

![](img/bbe6598e4d127a2e2cbe73681d4eb13a.png)

answer for the above query

如您所见，该查询返回的记录在姓氏中只有 5 个字符。

这个查询也可以写成这样

![](img/2f0c1dece32b2ab065bd8eccd848ba24.png)

query to obtain last_name having 5 characters

。可以用{n}代替重复(。)n 次

![](img/bbe6598e4d127a2e2cbe73681d4eb13a.png)

answer for the above query

> [ ] :

使用方括号来进一步优化模式匹配。

> 名字中包含特定字符的记录:

![](img/547521b7c9f463f13ddc683ba3169a2f.png)

query to return first_name containing x,y,z .

![](img/4d88d50f3bfd2181c8fa5c2046a45104.png)

answer for the above query

> 名字以特定字符开头的记录:

![](img/48316b1a03c9f502dee34864e800fabd.png)

query to return first_name containing starting with x,y,z

![](img/cf1e6335b40c8c987b32e1faa13cd21c.png)

answer for the above query

> 名字以特定字符结尾的记录:

![](img/e19283fd786c3483bd0e2d7abe8db87d.png)

query to return first_name containing ending with x,y,z

![](img/0037613013e718f6ddaccd544ac270a1.png)

answer for the above query

结论:

这篇文章包含了一些基本的正则表达式模式，希望这篇文章对你有所帮助。

谢谢你……