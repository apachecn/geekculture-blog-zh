# 每个数据科学家都应该知道的 24 条经验法则

> 原文：<https://medium.com/geekculture/24-maxims-every-data-scientist-should-know-d9ef9df5887e?source=collection_archive---------5----------------------->

## 简单易记的数据科学面试准则

![](img/8a4268170b443ae86d261021c39b52c9.png)

Image by Author.

每个人都喜欢经验法则。他们非常出色地将直觉封装在一个强有力的句子中，并传递出与你通读一整章书相同的价值。我通读了吴恩达的书， ***机器学习向往*** ，然后想出了下面这些言简意赅，容易记住的格言。

> 当你想修改一般的机器学习概念或准备数据科学面试时，请将这些经验法则放在手边。

本文假设你知道机器学习的基本概念。如果你想了解一条格言的其他内容，请参考下面的“其他内容”部分。

# 你应该知道的 8 个机器学习策略！

1.  ***比什么都重要，规模驱动机器学习进步*。无论是更深层次的神经网络还是获得更多数据，理论上，向上扩展都会提高性能。**
2.  ***战略性地解决性能瓶颈*。**大多数问题都会留下如何解决的痕迹。
3.  ***保证建模时数据的可分性*。**用于学习算法的训练数据，用于调整参数的开发数据，以及用于评估性能的测试数据。
4.  开发和测试数据应该来自同一个发行版。
5.  Dev 数据集应该足够大，以检测出您试图测量的最小算法差异* [1]。
6.  ***避免使用多个优化指标的诱惑*。**选择一个单一的数字优化指标，即易于快速跟踪、比较和迭代* [2]。
7.  即使你只能有一个优化指标，你也可以有多个满意指标* [3]。
8.  ***快速建立你的第一个系统，即使它并不完美*。**优化指标、误差函数、模型架构和超参数等决策可以在未来进行修改。

# 节省你时间的 8 个错误分析技巧！

1.  ***跨多个类别分组错误。快速浏览这些类别，凭直觉找出如何解决它们。***
2.  将需要较少努力的错误类别作为目标，并为整体绩效提供较高的回报。
3.  从偏差和方差的角度考虑错误分析简化了过程，并让您快速知道如何修复它们。* [4]
4.  低偏和**高**方差= *过拟合*
    **高**偏和低方差= *欠拟合*
    **高**偏和**高**方差= *过和欠拟合*
5.  得出一个最优的错误率，这是问题固有的不可避免的偏差* [5]
6.  如果你有高的可避免的偏差，增加你的模型的复杂性，并且在高方差的情况下增加你的数据量。
7.  采用正则化来减少因模型复杂性增加而引入的偏差。
8.  修改输入特征和架构以更好地适应问题将同时导致方差和偏差的减少。

# 处理肮脏的真实世界机器学习问题的 8 个事实！

1.  对于自动化人工任务的 ML 问题，人工表现是最佳的错误率。
2.  当性能低于最佳错误率时，进展会更快，并在超过人类阈值后逐渐减少。
3.  ***不是所有的数据都相等。*** 你的训练数据将包含不同的数据子组，与未来的数据分布具有不同程度的相似性。
4.  ***对与未来预期分布非常相似的数据进行加权*** ，重视预期数据，但也从其他可用数据中学习，尽管重要性较低* [6]。
5.  组合不同分布的数据并不总是明智的。 仅在数据较少时使用，或者您可以添加一个字段来通知它们来自不同的分布* [7]。
6.  **高方差可能会掩盖您的培训/开发数据集来自不同的分布**。通过添加更多相关示例或增加权重，使您的训练数据集类似于 dev，从而解决这个问题。
7.  除非你有一个简单的 ML 任务或者一卡车的数据，否则最好将 ML 问题分解成更小的 ML 任务。* [8]
8.  在一个 ML 流水线中，**预测仅仅和它的部分之和**一样好。确保每个组件都达到最佳的错误率。* [9]

# 附加上下文

为了使准则尽可能简洁，准则中不包括额外的上下文。如果需要，下面的列表提供了附加的上下文。

1.  如果您希望检测 0.1%的准确度变化，那么您应该至少有 10，000 个示例。 ***你需要检测的精度越低，你需要的例子就越多*** 。
2.  例如，您可以针对 F1 分数进行优化，而不是针对精确度和召回率进行优化，这是一种将二者结合起来而不是取平均值的更聪明的方法。 ***在有意义的情况下，将多个优化指标组合成一个数字*。**
3.  ***优化指标是算法试图优化的指标，满意指标是模型应该遵守的最小约束*** 。满足性度量的例子包括最小推断时间、模型大小、算法公平性等。例如，假设你有 90%和 92%准确率的模型 A 和模型 B，但是模型 B 有种族偏见。你最好选择 A 模式，因为 B 模式不符合避免种族偏见的令人满意的标准。
4.  偏差是训练错误率，方差是训练错误率和开发/测试错误率之差。
5.  偏见是可避免的和不可避免的偏见的总和。例如，在语音识别中，人类的准确性给了我们一个我们试图克服的最佳错误率。但是如果背景噪声很高，就不可能得到很好的精度。在这种情况下，最佳误差率大于零，这就产生了不可避免的偏差。*没有不可避免的差异*。您可以通过增加数据集大小来消除所有差异。
6.  假设你正在开发一个移动应用程序，它从用户上传的内容中检测猫。你可以有一个更小的数据集，它来自手机拍摄的照片，与未来的预期数据非常相似，但也有大量来自网上的包含猫的例子。你仍然可以从两者中学习到潜在的相似性，但是给手机上传的照片比网络废弃的照片更多的权重会导致一个更好学习的模型。
7.  例如，您有一个房价预测问题，您试图预测纽约州的房价。现在，您有了俄克拉荷马州房价的附加数据集。这是两个完全不同的数据集，如果您别无选择，只能合并它们，请添加一个地理位置字段。这给了模型编码它们不同的机会。
8.  如果我们正在研究一辆自动驾驶汽车，你可以有不同的子问题，这些子问题可以被设计为一个检测车道的模块，一个检测其他汽车的模块，以及一个检测人的模块。然后你想出一种方法，以一种有意义的方式将所有这些模块放在一起，执行无人驾驶汽车的 ML 任务。
9.  在自动驾驶汽车中，如果你有一个糟糕的车道检测系统，那么无论你的其他组件表现得多么好，你都会突然转向穿过车道并引发问题。

我是一名数据科学家，在🛢️石油和⛽.天然气公司工作我所有的✍️文章永远免费，不货币化💰。我希望没有媒体订阅的人仍然可以访问它。这是我回报社会给予我的一切的方式！。如果你喜欢我的内容，请关注我👍🏽在 [LinkedIn](https://www.linkedin.com/in/adiamaan-keerthi/) 和 [Medium](https://blog.adiamaan.com/) 上。订阅以便在我发帖时收到提醒。

![](img/d63433c32ae44e37c03fa53e5b7f7925.png)