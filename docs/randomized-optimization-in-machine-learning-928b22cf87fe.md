# 机器学习中的随机优化

> 原文：<https://medium.com/geekculture/randomized-optimization-in-machine-learning-928b22cf87fe?source=collection_archive---------14----------------------->

## 在这篇博客中，我将回顾一些随机化优化算法的基础知识。

# 贝叶斯的东西

贝叶斯概率用于机器学习领域，在高层次上理解贝叶斯规则是关键。

> 贝叶斯规则将一个事件的条件概率与实际情况联系起来。

在贝叶斯学习中，贝叶斯规则帮助我们在给定输入空间和一些领域知识的情况下找出最可能的假设。重要的是，这可以简单地表示为， *argmax Pr[h|D] h∈H* ，其中 D 是从某个概率分布中选择的输入空间， *h* 我们的假设是所有可能假设的集合， *H* 。应用贝叶斯规则将给出一个更友好的表达式，它将我们的假设输出我们正在寻找的东西的可能性、该输出的先验领域知识以及输入空间的概率分布联系起来。

在贝叶斯学习中，我们简单地将贝叶斯规则应用于所有可能假设的问题，并取 argmax。但是找到友好贝叶斯规则表达式中的一些参数可能是困难的。让我们试着回答一些问题。您可以使用最大后验概率( *MAP* 或最大似然( *ML* )假设来帮助找到假设的先验领域知识。ML 似乎是值得推荐的，因为它的最终表达式是简单的误差平方和，最小化它是找到一个好的假设的方法。

这些都可以归结为更深入的贝叶斯和概率理论，如贝叶斯网络、推理和朴素贝叶斯，它们是许多机器学习算法的基础，我在这里不会讨论。这里还有很多，我建议看一下我一直在读的笔记的第 4 章，因为看到方程是非常有帮助的，他给出了很棒的通用例子。

# 无监督学习:随机化优化

优化问题被给定一个输入空间、目标和目的，其中目的是为了在输入空间内找到最佳输出(即目的)的适应度函数。优化倾向于针对一些问题，例如找到算法的最佳过程、路线、根或参数。在现实世界中，输入空间可能是巨大的和复杂的函数，在这里很难找到解决问题的最佳方案，并且通常您最终会找到局部最优方案(*可能是下一个最佳方案*)而不是全局最优方案。因此，需要提供算法来搜索“最有可能”产生最佳答案的输入空间的随机化优化。

## 爬山

*   随机猜测一个起点，并在初始起点的局部邻域内搜索单个“最佳”输入。
*   很容易陷入局部最优，但你可以增加猜测的次数，以增加找到至少一个真正好的局部最优(可能是全局最大值)的机会——这就是*随机重启爬山算法。如果找到了局部最小值，我们就重新尝试从一个新选择的随机起点开始。*
*   依赖于输入空间的 *argmax* 。

## 模拟退火

*   基于随机爬山，模拟退火算法说，我们不一定需要不断猜测新的起点并爬上“山”，而是通过探索它们来改进我们已经做出的猜测。
*   在我们开始寻找一个新的邻居和探索那个邻居的次数之间找到一个平衡是关键。这给了我们更多找到全局最小值的机会
*   邻域的探索使用接受概率函数来确定摄取坏点的严重程度。这是由一个*温度*参数驱动的——当*温度*达到 0 时，该算法的行为就像爬山一样(导致局部最大值),当*温度*接近无穷大时，我们接受更多的值，不管它有多差(行为像随机走过)。
*   这一切都导致了一个性质，告诉我们在一个特定的输入结束的概率与它有多好成正比
*   通常从高温开始，然后慢慢降低(这称为冷却系数)
*   重复退火可能非常慢，这取决于成本函数的复杂程度。调整这种算法可能是一个微妙而繁琐的过程。

## 遗传算法

*   基于生物学，该算法着眼于变异(局部搜索)和交叉的种群(随机样本输入空间)，从而导致不同的种群组合“可能”在一定时间内产生更好的东西(改进的迭代)。
*   让这个算法不同于并行随机爬山的关键是交叉。首先，我们通过用适应度函数(*截断*或*轮盘赌策略*)评估值来迭代更新群体，然后发生交叉，以基于概率上的成对个体生成更“适合”的新群体，而不是仅取局部最大值。
*   该算法不太可能陷入局部最优，因为它不断地用其他值替换值(用后代替换*父代)，而不是平滑地从一个值移动到下一个值的梯度下降。*
*   将优化问题表示为遗传算法的输入是困难的。

## 模仿

*   MIMIC 希望更多地了解复杂的函数空间，而不是其中的一个点——它只是更关心。分解后，MIMIC 试图对潜在的概率分布进行建模，并随着时间的推移对其进行迭代改进。因此，与前面提到的任何算法不同，它“记住”输入空间的先前搜索。
*   概率分布考虑输入空间内被认为在适应度级别上“足够好”的均匀样本。分布的选择取决于阈值*θ，*当查看最小值*θ*时，分布在整个输入空间上是均匀的，当查看最大值时，它只查看函数的局部最优值。
*   MIMIC 从整个输入空间上的分布开始，并迭代地改进它，直到我们达到最优。每次迭代取数据集的某个增加的百分比，然后取“足够好”的值，以便估计下一个概率分布，直到我们满足用户选择的任何百分比停止标准。
*   MIMIC 假设概率分布是可以估计的，并且我们实际上选择了“足够好”的值来进行估计。这种估计是使用依存关系树来完成的，依存关系树是一种贝叶斯网络。简而言之，我们生成一个最小化成本函数(使用 *KL-divergence* ) 告诉我们任何两个分布之间的相似性)，然后由于引入了一个最大化特征之间互信息的新术语，该函数被最大化，这使我们能够创建一个连通图(其中节点是特征，边是特征之间的互信息),由于我们试图最大化互信息，所以边权重被反转，以便可以跨越该图，从而找到一个新的概率分布…
*   MIMIC 喜欢结构，因此当最佳值取决于结构而不是精确值时，MIMIC 将表现良好，这与不能由概率分布表示的适应度函数相反。由于特征之间关系的“存储”, MIMIC 通常比其他算法收敛的迭代次数少，但是可能需要更多的时间，并且需要更多的处理内存。

> 使用的参考:
> 
> [https://github . com/Mohamed ameen 93/CS-7641-机器学习-Notes/blob/master/UL01。% 20 random % 20 optimization . pdf](https://github.com/mohamedameen93/CS-7641-Machine-Learning-Notes/blob/master/UL01.%20Randomized%20Optimization.pdf)
> 
> [https://teapowered.dev/assets/ml-notes.pdf](https://teapowered.dev/assets/ml-notes.pdf)