# 在 ASP.Net 使用$compute 和$ search ODATA

> 原文：<https://medium.com/geekculture/using-compute-and-search-in-asp-net-odata-550f916e4ae2?source=collection_archive---------7----------------------->

![](img/6032fa2f8459a2a9580f3c7446eed8df.png)

Photo by [https://www.digitalonus.com/](https://www.digitalonus.com/) ([src](https://www.digitalonus.com/wp-content/uploads/2020/12/netcore.jpg))

新的 ASP.Net 核心 OData 包现在包含两个主要变化。它现在将支持以下两个新的查询选项

*   [$计算](http://docs.oasis-open.org/odata/odata/v4.01/odata-v4.01-part2-url-conventions.html#_Toc31361047)
*   [$搜索](http://sec_SystemQueryOptionsearch)

与其他查询选项一起，上面两个选项将在服务器端执行一组转换来控制结果数据。所以你可以在服务器上修改/计算数据，而不是…