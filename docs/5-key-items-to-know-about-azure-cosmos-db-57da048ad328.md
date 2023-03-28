# Azure Cosmos DB 的 5 个关键元素

> 原文：<https://medium.com/geekculture/5-key-items-to-know-about-azure-cosmos-db-57da048ad328?source=collection_archive---------18----------------------->

让我们先介绍一下微软 Azure 的完全托管的 NoSQL 数据库——Azure Cosmos DB。

![](img/958cf9c9fc7a4f3c13d53fa1d4a82185.png)

Photo by [Scott Graham](https://unsplash.com/@homajob?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/cloud-computing?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Azure Cosmos DB 具有以下(但不是全部)优势:

1.  任何规模的保证速度
2.  关键任务就绪(确保 99.999%的可用性和企业级安全性)
3.  全面管理且经济高效(自动扩展和端到端数据库管理)

*您可以在这里* *阅读更多详情和其他优惠* [*。*](https://docs.microsoft.com/en-us/azure/cosmos-db/introduction)

## **Azure Cosmos DB 有哪些可用的容量模式？**

**无服务器** 基于消耗，只对数据库操作和存储消耗的请求单元(ru)收费。这对于您还不知道用途的原型或小型应用程序来说可能是好的。

**调配吞吐量** 您可以在数据库级别或容器级别应用。即使您没有使用，也会按照您提供的金额向您收费。如果您的应用程序对数据库有稳定的高消耗需求，建议您这样做。

请注意，两种不同模式的存储/速率限制可能不同。我在下面强调了一些关键的问题。

还不确定选择哪种模式？看看微软的这个 [*例子*](https://docs.microsoft.com/en-us/azure/cosmos-db/throughput-serverless) *。*

## **什么是分区键？**

Azure Cosmos DB 需要一种方法来“切割”我们的数据，以便进行缩放，它使用您在此操作中选择的分区键。

分区键告诉 Azure Cosmos DB 如何将数据分组到一个`Logical Partition`中，因此同一逻辑分区中的所有项目都具有相同的分区键值。例如，在一个超市场景中，一个容器容纳它出售的所有类型的商品。每个项目都有项目 Id 和项目类型(如水果、蔬菜或饮料)。如果选择 `*itemType*` *作为分区键，所有水果项将在一个逻辑分区中，所有蔬菜项将在另一个逻辑分区中，以此类推。*

您必须在容器的供应阶段预先指定分区键，此后不允许进行任何更改。任何改变都需要数据迁移，这将是另一个话题。分区键的选择将影响 cosmos db 的性能。因此，在开始 Azure Cosmos DB 之旅之前，理解分区键应该考虑的不同因素是非常重要的。不要担心，我会分享另一篇关于一个人必须考虑的关键因素的文章！

## **关于逻辑分区的更多信息**

逻辑分区由一组具有相同分区键的项目组成。每个条目都有一个 id，该 id 只能在逻辑分区中是唯一的。*以同一个超市为例，你可以有一个 id = 1，itemType =水果的商品，另一个 id = 1，itemType =蔬菜的商品。*

数据库事务的范围位于同一个逻辑分区内。因此，如果您的应用程序需要批量操作，或者您可能需要运行存储过程/触发器，您必须仔细考虑需要执行的项目，并尝试将它们组合在一起(*这是考虑的因素之一*)。

虽然容器中的逻辑分区数量没有限制，但是每个逻辑分区的最大存储容量为 20GB ( *这是另一个考虑因素*..

## **关于物理分区的更多信息**

您可能到处听说过物理分区，但它可能非常简短。基本上，物理分区是 Azure Cosmos DB 服务器的实际物理存储。容器通过在物理分区之间分布数据(逻辑分区)和吞吐量来扩展。以下是决定物理分区数量的两个因素:

1.  总数据存储量。物理分区的最大存储容量为 50GB。
2.  调配的吞吐量数量。物理分区的最大吞吐量为 10，000 RU /秒。因为逻辑分区被映射到物理分区，所以逻辑分区也具有相同的吞吐量限制。

就像逻辑分区一样，容器可以拥有的物理分区的数量没有限制。Azure Cosmos DB 是一个托管数据库，我们不需要担心它，因为这些是内部实现，由 Azure Cosmos DB 本身控制。我们所要关心的是分区键，因为它被用来分配数据和吞吐量。分区键决策不当可能会导致热分区——当大多数请求被重定向到一小部分分区时，会导致调配的吞吐量使用效率低下(如果这是您选择的计划)、速率受限，当然还有高成本。

## **限制**

除了我在前面几节中分享的逻辑和物理分区的标准最大存储大小之外，这里还有一些其他限制，根据您选择的消费计划，这些限制可能会有所不同。

每个请求的限制

*你可以在这里* *找到更多关于其他极限* [*。*](https://docs.microsoft.com/en-us/azure/cosmos-db/concepts-limits)

## 摘要

![](img/11585d299d8600f7a21498a1993ab526.png)

Overview of Azure Cosmos DB Components (Shared throughput by container)

上图描述了所有不同组件的概要，以及它们在 Azure Cosmos DB 中的连接方式。它演示了在容器共享吞吐量的情况下，逻辑分区到物理分区的映射。但是，如果改为在数据库上设置吞吐量，映射可能会有所不同，因为物理分区可以在不同的容器之间共享。

这篇文章在很高的层次上介绍了 Azure Cosmos DB，我希望这些核心信息可以帮助您开始为应用程序设计数据库。感谢您的阅读，敬请关注下一篇关于选择[分区键](/@mariochiadev/how-to-select-a-partition-key-in-azure-cosmos-db-partition-key-strategies-f44ba134790e)的文章！如果你喜欢这篇文章/觉得它有帮助，如果你能为我鼓掌或喜欢这篇文章以示支持和喜爱，我将不胜感激。

## 参考

[微软官方文档](https://docs.microsoft.com/en-us/azure/cosmos-db/)