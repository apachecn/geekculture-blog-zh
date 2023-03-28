# 数据工程:探索数据管道和数据仓库背后的关键概念

> 原文：<https://medium.com/geekculture/all-about-data-engineering-exploring-the-key-concepts-behind-data-pipelines-and-data-warehouses-d366a55ca14e?source=collection_archive---------9----------------------->

![](img/9ead4b4f874a59a9d9c9e626ebbb88e6.png)

在 CapeStart，我们以帮助初创公司和财富 1000 强公司满足他们的数据工程需求而自豪，从构建 ETL/ELT 数据管道到开发数据仓库和数据湖。

但这也意味着我们经常收到客户和潜在客户关于数据工程世界某些方面的问题。

所以，我们想，为什么不写一篇博文来解释一些关于数据工程的最常见的术语呢？

让我们开始吃吧。

## 数据工程

数据工程是工具、过程和操作的集合，它便于数据从数据源流向组织内的目标系统和业务用户。数据工程是由数据工程师实现的，他们负责构建和维护组织的数据基础设施。

## 数据工程师

顾名思义，[数据工程师](https://searchdatamanagement.techtarget.com/definition/data-engineer)是实现数据工程的 IT 人员。数据工程师的存在是为了确保组织的数据始终具有最高的质量和可用性。

数据工程师通过构建将各种数据源连接到目标系统(如数据仓库)的数据管道来实现这一目标。这包括集成、清理和结构化传入和存储的数据，以确保数据科学家和业务用户执行的大数据分析和其他应用程序的高可用性。

虽然大多数数据工程师都有自己独特的技能组合，包括管道和数据库构建，但这份工作还需要融合软件工程和数据科学中的其他技能，包括 Python、SQL 和 Java 等编程语言的知识。

一些数据工程师负责从收集到处理的整个数据生命周期，而其他人则专门负责构建和维护数据管道或数据库。

## 数据管道

想象一下一条能源管道将石油或天然气从一个设施运送到另一个设施，你就对数据管道的作用有了很好的了解——不同之处在于它们是为数据而不是物理材料而构建的。数据管道通常通过由数据工程师构建和管理的一套工具和流程，在源位置和目标位置之间移动数据，如物联网传感器和数据湖。

数据管道[需要](https://searchdatamanagement.techtarget.com/definition/data-pipeline)开发环境(用于构建、测试和部署管道)和工具来监控管道健康状况(包括检查数据管道架构中的错误)。

[智能数据管道](https://www.technologyreview.com/2021/12/06/1040716/evolution-of-intelligent-data-pipelines/)尽可能地使用自动化来扩展许多组织正在生成的数量急剧增加的数据、数据源和数据类型的接收。

## ETL/ELT

Extract-transform-load (ETL)和 extract-load-transform (ELT)略有不同，但都是使用数据管道将数据从源系统引入目标系统的类似方法。

使用 ETL 工作流，数据工程师首先从数据源接收输入数据(提取)。接下来，他们修改并集成数据，使其标准化，并使其可供分析师使用(转换)。他们将数据存储在数据仓库或其他类型的数据存储(load)中，以便业务就绪并可供使用。

[另一方面，](https://solutionsreview.com/data-integration/etl-vs-elt-and-the-benefits-of-data-transformation-in-the-cloud/)ELT 工作流在进行任何转换之前将数据加载到目标系统(通常是云数据仓库)中。然后，使用数据仓库本身的计算能力按需执行数据转换。

由于上述原因和其他原因，ELT 倾向于以更低的成本实现更好的性能，因为它只转换业务用户所需的数据——而不是像 ETL 那样转换大量的传入数据。ELT 有助于提高开发效率，降低基础设施的复杂性，并允许 IT 团队运行更快的数据作业。

## 数据仓库

数据仓库是一个中央数据存储库，通常是一个关系数据库，驻留在本地或云中(或者在混合环境中同时驻留在本地和云中)。它存储结构化数据。

数据仓库(以及数据集成等数据工程活动)有助于消除[数据孤岛](https://www.capestart.com/resources/blog/avoid-silos-with-data-management-integration-a-cloud-data-warehouse/)，并作为包含组织所有数据的单一真实来源，实现更准确的数据分析、更清晰的见解和更好的业务决策。

数据仓库由不同的层组成，包括分析层、语义层和数据层。它们还包括几个[基本组件](https://searchdatamanagement.techtarget.com/definition/data-warehouse)，包括:

*   **存储**。数据仓库必须能够存储一个组织的所有数据，并使其可供业务用户使用。存储类型从内部服务器到云服务，如 AWS 上的[云存储](https://aws.amazon.com/products/storage/)、[谷歌云存储](https://cloud.google.com/storage)和 [Azure Blob 存储](https://azure.microsoft.com/en-us/services/storage/blobs/)。
*   **ETL/ELT 工具**。请参见上面的部分。
*   **元数据**。为了使数据可搜索并对分析查询有意义，它必须有元数据。被一些人描述为“关于你的数据的数据”，业务元数据给数据集增加了上下文。技术元数据表示数据的结构以及数据存储的位置。
*   **数据访问工具**。这些工具允许数据科学家、业务用户和其他人访问和使用数据进行分析或其他应用，包括查询和报告工具以及数据挖掘工具。

需要注意的是，数据仓库不同于传统数据库，传统数据库通常只存储来自一个数据源(而不是多个数据源)的数据。与传统数据库相比，数据仓库通常具有更少的表和简化的模式和查询，从而在更大的数据集上实现更好的性能。

## 数据集市和数据湖

数据湖使用 ELT 方法来集成数据，并存储来自物联网设备、移动应用程序和网站等来源的大量典型半结构化或非结构化数据。因为在为数据湖捕获数据时不需要预先定义规则和模式，所以在处理非结构化数据时，它们允许更大的灵活性和性能。

数据集市本质上是规模小得多的数据仓库(通常为 100 GB 或更小)，通常专注于一个主题或一条业务线。数据集市可以帮助大型组织中的用户更快、更容易地找到和使用特定于他们部门的数据，同时所有数据仍然连接到更大的数据仓库(以防止数据孤岛)。

## 让 CapeStart 成为您的数据工程和数据仓库合作伙伴

CapeStart 的[数据工程](https://www.capestart.com/services/big-data/data-engineering/)、[数据科学](https://www.capestart.com/services/big-data/data-science/)、大数据和数据仓库团队每天都与大大小小的组织合作，指导他们的数据工程工作——从定制 ETL/ELT 管道到数据仓库创建和迁移。[联系我们](https://www.capestart.com/services/big-data/mlops/free-trial/)与我们的技术专家进行一次简短的发现通话。