# 机器学习方法简介

> 原文：<https://medium.com/geekculture/a-brief-introduction-to-machine-learning-methods-ffad53cefe41?source=collection_archive---------22----------------------->

"如果你不能向一个六岁的孩子解释，那你自己也不明白。"
― **阿尔伯特·爱因斯坦**

![](img/22cf76135df4179dfb16d415a46659fd.png)

Photo Credit: [Greg Rakozy](https://unsplash.com/@grakozy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

今年春天，我有幸在斯坦福大学上了一堂机器学习课，老师是谷歌的数据科学家查理·弗拉纳根。这门课的任务是“降低在日常商业环境中使用人工智能技术的门槛”，它确实做到了这一点。他解释说，“随着人工智能技术的激增，它们正成为希望保持竞争优势的企业的优先事项。释放业务数据力量的工具和技术已经变得非常普及。”本着同样的精神，我对下面的课程做了一个简要的回顾。让我们开始吧…

**线性回归**可能是机器学习(ML)最简单也是最广为人知的方法。这是一种从数学上找出哪些变量对特定结果有影响的方法。它回答了这样的问题:如果我改变“x ”,这会如何影响“y ”?哪些因素最重要，哪些因素可以忽略？这些因素是如何相互作用的？一个挑战:过度拟合。一种解决方案是:将数据集分成几部分。首先是训练数据集，即用于拟合模型的数据样本。接下来，在调整模型时，验证数据集或用于提供模型的无偏评估的数据样本符合训练数据集。最后，测试数据集或用于提供最终模型在训练数据集上的无偏评估的数据样本。谈到回归，有时我们需要以最有意义的方式缩放输出(即，答案应该限于是(1)或否(0)或介于两者之间)。在这些情况下，我们使用一个**逻辑回归**模型将数据点转化为适当的比例。当有多个变量时，我们还可以使用一个称为**梯度下降**的非线性过程，该过程从每个系数的随机值开始，并通过迭代最小化训练集模型的误差来优化它们。

接下来是基于树的模型。这些比你想象的更常见，每天都被用于决定事情，比如谁获得了汽车或房屋贷款的批准，通过一系列具有二元结果的级联问题(很像树的分枝)。**随机森林**或随机决策森林通过在训练时构建大量决策树并输出单个树的平均值/平均预测来运行。这些由多个单独的树组成，每个树都基于训练数据的随机样本。每个随机森林树基于一个随机样本，并且在每个节点处，考虑一组随机的特征来进行分割。随机森林可以非常快速有效，并且能够处理丢失的数据。但是，它们通常无法预测超出训练数据范围的数据，并且倾向于过度拟合噪声特别大的数据集。任何 ML 模型的性能也会逐渐下降，需要随着时间的推移重新训练。

然后，我们讲述了自然语言处理的**(NLP)。NLP 的最终目标是以一种有价值的方式阅读、破译、理解和理解人类语言。如今，这项技术被应用于从语音识别到聊天机器人的方方面面。谷歌有一个名为 AutoML 自然语言的工具，它使用机器学习来分析书面单词的结构和含义。它可以训练一个定制的机器学习模型来分类文档，提取信息，甚至理解作者的情绪(仍是一项正在进行的工作)。我们还用来自 OpenAI 的 GPT-3(预训练生成变压器 3)进行了实验，open AI 是一个人工智能研究组织，由山姆·奥特曼和埃隆·马斯克(后来因担心该技术的安全性而辞职)共同创立。GPT-3 使用预先训练的算法生成文本，通过在浩瀚的互联网上爬行，包括维基百科的全部文本。如果你问它一个问题，你会得到一个详细的答案。如果你友好地请求，它甚至可以写一首诗…**

****命名实体识别** (NER)是一种信息提取技术，它自动识别文本中的命名实体并将它们分类到预定义的类别中。实体可以是人、组织、位置、时间、数量、货币值、百分比等名称。NER 通过总结描述性文本、评论和讨论来提高搜索结果和推荐的速度和相关性。单词嵌入是一种单词表示形式，它将人类对语言的理解与机器的理解联系起来，并被我们日常使用的许多应用程序使用，如 Doordash 和 Airbnb。我们还讨论了推荐系统，比如那些用于根据用户行为模式推荐电影或约会资料的系统(例如，相似的 swipees 被聚集在一起)。输入是用户查询，输出是大小等于项目数量的概率向量，表示用户将与每个项目交互的概率；例如，他们刷约会档案或选择电影的概率。一个著名的例子是网飞，几年前举办了一场比赛，为任何能够使用来自 480，189 名客户的 17，770 部电影的超过 1 亿个评级的数据集，将其推荐引擎的准确性提高 10%的人提供 100 万美元的奖金。具有讽刺意味的是，网飞推荐引擎已经非常准确，他们甚至没有使用获胜的代码，因为它“似乎没有证明将它们引入生产环境所需的工程努力是合理的”**

**最后，我们讨论了最激动人心的话题之一:**神经网络**。本质上，它们是基于模拟连接的“神经单元”的人工智能系统，松散地模拟了神经元在大脑中的交互方式。自 20 世纪 40 年代以来，科学家们一直在研究受神经连接启发的计算模型，但最近随着计算机处理能力的大幅提高，这些模型越来越受欢迎。人工智能从业者将这些技术称为“深度学习”，因为神经网络有许多(“深度”)层模拟的相互连接的神经元。在更复杂的一端，有**卷积神经网络**(CNN)，其中神经层之间的连接受到视觉皮层组织的启发，视觉皮层是大脑处理图像的部分，非常适合感知任务。我们邀请了一位来自 Alphabet 自动驾驶部门 Waymo 的演讲嘉宾，向我们展示这是如何实时工作的。另一种被称为**强化学习**的技术通过分配虚拟的“奖励”或“惩罚”来训练系统，本质上是通过试错来学习。谷歌 DeepMind 使用强化学习来开发可以玩游戏的系统，在某些情况下甚至比最受好评的人类玩家更好。尽管看起来令人生畏，但归根结底还是要找到一个适合数据的函数。像谷歌的 TensorFlow 这样的工具也让神经网络变得更容易访问，几乎不需要编码。最近低代码/无代码 AI 模型激增。还有**生成对抗网络** (GANs)，Deepfakes 背后的“黑魔法”，可以学习模仿各种数据分布(例如文本、语音和图像)，因此既强大又潜在危险。**

**机器学习并不都是美好的，模型也不是天生客观的。我们通过向模型提供一组训练样本来训练它们，而人类参与这些数据的管理会使模型容易产生偏差。因此，在构建模型时，了解数据中可能出现的常见人类偏差非常重要，这样您就可以采取主动措施来减轻其影响。这就是为什么人工智能伦理领域在今天如此重要。我们仍然处于意识到 AI/ML 真正能提供什么的早期阶段。像任何工具一样，最重要的部分是我们如何使用它！**

**感谢您阅读本课程总结！要了解更多，我建议查看斯坦福大学秋季课程目录，了解即将推出的课程。**

**![](img/dba04527cb30d481e112669e0eca75d0.png)**

**Photo Credit: Stanford University**