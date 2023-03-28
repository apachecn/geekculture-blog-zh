# 幂等数据流水线

> 原文：<https://medium.com/geekculture/idempotent-data-pipeline-ba4c962d8d8c?source=collection_archive---------0----------------------->

## 使您的数据管道可靠

《牛津词典》中定义的幂等元是一个集合中的一个元素，当它自身相乘或运算时，其值不变。

您可能会问，这与我的数据管道有什么关系？帮我拿着啤酒，好吗？

![](img/3f41880122efe68acbb928bcdd5a9c16.png)

Image from [Thesaurus](https://thesaurus.plus/thesaurus/idempotent)

# 大纲:

1.  什么是幂等数据管道
2.  幂等数据管道的优势
3.  如何使数据管道幂等
4.  结论

## 1.什么是幂等数据管道

多次运行从源获取数据并将其加载到关系数据库中的管道可能会导致数据库中出现重复值，从而导致错误的度量和许多其他错误。使管道幂等将防止这一点，并使你成为一个更好的工程师。

换句话说，使用相同的输入多次运行数据管道将始终产生相同的输出。

## 2.幂等数据管道的优势

下面列出了幂等数据管道的一些优点

*   这确保了在回填的情况下不会在存储位置产生重复的数据
*   它使得管道中的转换结果是可预测的
*   它有助于减少数据存储费用
*   它还有助于删除旧的/不需要的数据

## 3.如何使数据管道幂等

数据管道中最常见的步骤是

*   从一个或多个来源提取数据
*   执行一些转换
*   加载到数据仓库

幂等管道将确保，如果在列出的步骤中出现任何错误，仍然会产生预期的结果作为输出。

如果在加载阶段出现错误，数据没有完全加载到数据仓库中，我们的幂等管道应该从数据仓库中删除半加载的数据，并在管道重新运行时将新数据作为完全加载的数据存储。只有当数据管道重新运行时将产生所需的相同数据时，才建议这样做。这种模式被称为*删除-写入*模式。Spark、Snowflake 等技术提供了其他幂等设计模式，如[*Spark-overwrite*](https://sparkbyexamples.com/spark/spark-overwrite-the-output-directory/)*。*

下面提供了一种在 python 中实现*删除-写入*模式的方法:

delete_write_idempotent pipeline

在上面的代码片段中，我实现了一个简单的 ETL 管道，它从 s3 桶中获取 parquet 文件，并使用 pandas *read_parquet 来读取它。*在我的例子中，数据随后根据识别的业务逻辑进行转换，删除所有 *customer_id* 等于*的行。**delete _ write*模式在 ETL 的最后部分实现。这里， *load_dwh* 函数接受两个参数 *dataframe* 和**output _ location，*检查 *output_location* 是否已经存在，如果加载数据时出现错误，则删除 *output_location* 中指定的文件夹，并用新数据重新创建它。*

## *4.结论*

*拥有一个幂等数据管道可以避免数据工程师的许多头痛，特别是当数据管道由于错误或业务逻辑改变而需要多次重新运行时。*

## *参考和进一步阅读*

*Startdataengineering 有一篇关于[制造数据管道幂等元](https://www.startdataengineering.com/post/why-how-idempotent-data-pipeline/)的文章*

*Fivetran 解释了数据管道中的[幂等性](https://www.fivetran.com/blog/what-is-idempotence)*

*[幂等性及其如何防止数据管道故障](https://www.fivetran.com/blog/idempotence-failure-proofs-data-pipeline)five tran*