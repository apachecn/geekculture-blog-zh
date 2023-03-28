# 你为什么应该对 AlphaFold 感到兴奋？

> 原文：<https://medium.com/geekculture/why-should-you-be-excited-about-alphafold-3255293f0242?source=collection_archive---------37----------------------->

提示:这不仅仅是关于人工智能驱动的计算生物学。

![](img/8262695760cb12df8441fe7bba9a58f7.png)

Photo by [Sharon McCutcheon](https://www.pexels.com/@mccutcheon?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/a-kid-with-multicolored-hand-paint-1148998/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

一切都处于蜕变状态。你自己处于永恒的变化和腐败之中，无法与之对应；整个宇宙也是如此。——马库斯·奥勒留

Alphabet 的 DeepMind 自 2016 年以来所做的研究的最近高潮是 [**AlphaFold**](https://deepmind.com/blog/article/alphafold-a-solution-to-a-50-year-old-grand-challenge-in-biology) 。**这是一种机器学习方法，可以仅根据氨基酸序列预测蛋白质将采用的三维结构。现在让我把这个陈述分解成容易理解的部分。**

当你在学校看到平衡食物图表时，总会有一个叫做蛋白质的主要角色。你被告知蛋白质对你的健康非常重要，吃肉、蛋或豆类会神奇地给你更好的皮肤、更多的血液和更多的力量。无论是血红蛋白、胰岛素、胶原蛋白，还是那些强大的抗体，它们都只是不同类型的蛋白质，在给你一个健康的身体方面做得很好。所有这些蛋白质都由 21 个氨基酸组成，每个氨基酸都是碳、氧、氮和氢的某种组合。要了解原子是如何形成世界万物的，请看下图。

![](img/0d0579427d3522e1216ffe4c782a2325.png)

Source: The author

现在我们知道只有 21 种已知的氨基酸存在，那么它们是如何形成数百万种蛋白质的呢？

> 答案在于一串氨基酸是如何折叠起来形成环和卷，并包装成三维结构，然后变成可用形式的蛋白质。换句话说，蛋白质折叠赋予了它存在的本质。

所以如果我知道 AAs 的序列，那么我能预测蛋白质的预期结构吗？这位亲爱的读者在过去 50 年里一直是一个激烈的研究问题，被称为**蛋白质折叠问题**，AlphaFold 已经能够以很高的准确度回答这个问题。你可以在这里阅读更多关于蛋白质折叠问题[。](/@aryanmisra/the-protein-folding-problem-dfa49030f775)

通过计算生物学解决蛋白质折叠问题的意义有很多优点。首先，它将通过用更快、更便宜的计算机模拟代替缓慢、昂贵的结构生物学实验来加速药物发现。想象一下，为了治疗不同阶段的疾病，可以以极快的速度进行所有的排列。第二个目标是**帮助从基因组序列中解释蛋白质的功能**。现在基因组测序取得了突飞猛进的进展，破译蛋白质的密码并标记其功能对于进一步的研究非常有用。随着更多被识别的结构被添加到[蛋白质数据库(PDB)](https://www.rcsb.org/) ，蛋白质结构预测因此成为机器学习应用的一个伟大用例。

> “我们能预测蛋白质如何折叠吗？在几乎无限的可能折叠方式中，蛋白质仅在几十微秒内就选择了一种。同样的任务需要 30 年的计算机时间”

**AlphaFold 已经能够识别大约 100，000 种独特蛋白质的结构。虽然这只是存在数十亿已知蛋白质序列的冰山一角，但令人兴奋的是，它已经能够识别早期蛋白质数据库中不存在的序列。**

现在，一个黄金问题来了:AlphaFold 是如何取得如此有希望的结果的？

当我们谈到旨在**预测三维蛋白质结构的计算方法**时，他们遵循两种不同的方法。**第一种方法受到物理相互作用的影响。毫无疑问，水滴总是呈圆形，花粉种子的运动或蜜蜂在水面上的舞蹈都可以用物理学来解释。它们毕竟是动力学或热力学驱动力的结果；类似地，一个原子链如何折叠并达到某个三维形状也可以归因于这种力量。将它们转化为统计近似值的能力增加了这种方法的吸引力。不幸的是，它充满了挑战，如分子模拟的计算复杂性和产生足够精确的模型的困难。**进化历史的另一种方法**因此成为一种选择。由于基因组测序，已知结构、相关性推导和生物信息学范围的使用已被用于确定蛋白质结构的限制。尽管有这些进步，当代基于物理和历史的进化方法产生的预测缺乏实验准确性。**

AlphaFold 结合了基于蛋白质结构的物理、进化和几何约束的新型神经架构和训练程序。我将把 AlphaFold 网络的新颖性归纳为以下几点。

*   输入包括一级 AA 序列和通过[多序列比对(MSA)](https://en.wikipedia.org/wiki/Multiple_sequence_alignment) 比对的相似链序列
*   输出包括给定蛋白质的所有重原子的三维坐标预测
*   AlphaFold 网络有两个主要阶段:进化模块和结构模块
*   Evoformer block 将蛋白质结构预测视为三维空间中的图形推理问题。它使用关于输入序列之间的空间和进化关系的直接推理
*   结构模块以旋转和平移的形式为蛋白质的每个残基引入了明确的三维结构
*   整个 AlphaFold 网络和结构模块通过将最终损失应用到输出并将输出反馈到相同的模块来使用迭代改进
*   使用蛋白质数据库(PDB)数据进行监督学习
*   学生自我[蒸馏](https://towardsdatascience.com/knowledge-distillation-simplified-dd4973dbc764)被用于通过从[unicluster 30](https://uniclust.mmseqs.com/)制作预测结构的新数据集来提高准确性。使用 PDB 和增强的新数据集的混合来训练相同的模型。这种自蒸馏过程利用了未标记的序列数据

像 AlphaFold 这样能够准确预测蛋白质结构的系统可以加速许多对社会重要的研究领域的进展。它已经被用于被忽视疾病药物倡议(DNDi)(T7)，该倡议推进了他们对拯救生命的疾病疗法的研究，这些疾病不成比例地影响着世界上的贫困地区。此外，[CEI 朴茨茅斯大学的酶创新中心](https://www.port.ac.uk/research/research-centres-and-groups/centre-for-enzyme-innovation)正在利用 AlphaFold 的预测来帮助设计更快的酶，以回收一些污染最严重的一次性塑料。

科罗拉多大学博尔德分校的一个团队发现了利用 AlphaFold 预测研究抗生素耐药性的前景，而加州大学旧金山分校的一个小组已经利用它们增加了他们对新型冠状病毒生物学的了解。

来源:

*   [用 AlphaFold 进行高度精确的蛋白质结构预测](https://www.nature.com/articles/s41586-021-03819-2)
*   [将 AlphaFold 的力量交到世界手中](https://deepmind.com/blog/article/putting-the-power-of-alphafold-into-the-worlds-hands)