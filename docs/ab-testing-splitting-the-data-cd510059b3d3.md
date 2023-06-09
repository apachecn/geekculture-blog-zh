# AB 测试:拆分数据

> 原文：<https://medium.com/geekculture/ab-testing-splitting-the-data-cd510059b3d3?source=collection_archive---------8----------------------->

![](img/b758a71407a2c0e74d03252214601ad6.png)

# 问题？我可以 AB 测试，但是我需要先拆分我的数据！

AB 测试可能是数据驱动商业决策的最具标志性和最直观的用途之一。有大量的资源可以学习如何进行 AB 测试，通常是使用预先标记的“A”和“B”数据。但是，当我想在现实生活中实现 AB 测试时，我碰壁了:

我们如何决定哪些用户在浏览网站时显示“A”版本或“B”版本？

我们如何以随机的方式分割用户？

我们如何分割用户，以便在他们每次登录时显示相同版本的 AB 测试？

如何分割用户，使我们能够执行 ABC 测试，并继续测试新版本？

# 解决办法？桶和散列！

不是实体的，而是数字的。让我们想一想，如果我们有两组客户，我们可以很容易地将他们对半分。我们可以简单地将这些桶标记为 A 和 B，但是我们也可以创建更多的桶，允许我们以不同的比例划分，将 C 添加到测试中，或者甚至非常容易地执行新的测试。通过将客户分成 24 个桶，我们可以将他们分成 2 组、3 组、4 组等。

我们也可以保留一些桶来观察复合测试的长期效果。

我们正在寻找一种方法，将我们的用户唯一标识符(用户 id)从字符串转换为 1 到 24(或 0 到 23)之间的整数。

如果用户 id 已经是一个整数，那么使用模数运算(以 24 为基数)将用户“映射”到 24 个不同的组就很简单了。假设用户 id 是随机的，那么结果也将是随机的。这也将确保用户每次访问网站时总是被分配到相同的存储桶。

将字符串转换为整数的一种方法是使用哈希。在相当高的层面上，散列是…函数。他们接受输入并把它们映射到另一个维度。它们也是确定性的，所以它们将保持我们的结果一致。对于我们正在做的事情的所有意图和目的，它们也是“随机的”(在结果散列中不应该有一个模式)。这是一个好消息，即使原始用户 id 中有某种模式！

在 python 中，用户可以获取他们的用户 id 字符串，然后:

1.  用 utf-8 编码，
2.  对结果使用散列函数(例如 Sha1)，
3.  然后，将编码的数据表示成字节(例如使用。hexdigest())，
4.  将字节表示转换为整数表示(使用 int())，
5.  最后，使用模数运算将整数映射到 24 个桶中的一个(或者任意多个)(使用%)。

它看起来像这样(使用 sha1 散列函数):

```
int(hashlib.sha1(user_id.encode("utf-8")).hexdigest(), 16) % 24
```

# 最后的话

就是这样！现在，每个用户将由一个从 0 到 23 的随机整数来表示。人们可以很容易地构建桶和条件语句的列表，以向每个客户显示期望的输出。

这些也是快速计算的，因此它们允许在运行中分割用户，同时一致地向相同的用户显示相同的版本。

享受你的 AB 测试！