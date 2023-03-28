# 有效的 Java:考虑使用定制的序列化形式

> 原文：<https://medium.com/geekculture/effective-java-consider-using-a-custom-serialized-form-b3fa56d0edc8?source=collection_archive---------19----------------------->

![](img/1356dc6c99ee541250e7611b29ece54b.png)

Photo by [Mark Tryapichnikov](https://unsplash.com/@markinzone?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

正如我们在[之前的文章](/codex/effective-java-implement-serializable-with-great-caution-df123eb51cdf)中所讨论的，对象的序列化形式是其 API 的一部分。这意味着我们应该在未来的一段时间内尊重它，如果我们违反了它，我们将会给代码的用户带来不必要的负担。在这种情况下，我们应该非常小心地确定我们的类的序列化版本采用什么结构。那就是…