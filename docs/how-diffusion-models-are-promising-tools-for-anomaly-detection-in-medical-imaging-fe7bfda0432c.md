# 扩散模型如何成为医学成像异常检测的有效工具

> 原文：<https://medium.com/geekculture/how-diffusion-models-are-promising-tools-for-anomaly-detection-in-medical-imaging-fe7bfda0432c?source=collection_archive---------10----------------------->

![](img/fac8753a9ecd59ca12887d86bc5df54d.png)

众所周知，机器学习模型擅长检测模式，做出决策，并根据之前学习的训练数据做出其他有区别的决策。

但是一种新型的机器学习(ML)模型正被用来处理越来越多的用例。我们正在谈论[生成模型](https://developers.google.com/machine-learning/gan/generative)。

生成模型不同于判别模型，判别模型包括决策树、随机森林和逻辑回归。顾名思义，判别模型区分各种数据实例，并且在大多数情况下，基于该数据做出相对简单的是/否决策

生成式模型使用更多的数据点从相同的训练数据集生成全新的数据。

因此，不是简单地找出一棵树和另一个物体之间的区别，生成模型可以找出更复杂的关系。这些可能包括相关性，例如“树木可能会出现在自然场景中”或“树木超过 10 英尺高，而灌木较小。”

虽然生成模型有很多有前途的应用，但它们也有一个明显的缺点:它们是所谓的[“深度伪造”图像和视频内容](https://deepai.org/publication/towards-the-detection-of-diffusion-model-deepfakes)背后的技术。

但是这和检测脑瘤有什么关系呢？我们会在下面解释。

## 生成模型的类型

第一种生成模式是生成对抗网络()。由蒙特利尔大学的研究人员在 2014 年开发的 GANs 由一对决斗神经网络组成——因此得名“对抗性”——它们以创建新的合成数据的名义进行战斗。

GANs 非常适合合成语音、视频或图像生成应用。尽管 gan 经历了一个显著的繁荣期，并且特别适合多种应用，[它们已经开始趋于平稳](https://venturebeat.com/ai/how-diffusion-models-unlock-new-possibilities-for-generative-creativity/)。这在很大程度上是因为他们通常很难训练，容易出现模式崩溃，并且通常缺乏输出多样性。

其他类型的生成模型包括变分自动编码器(VAE)、基于流的模型和自回归转换器。特别地，后者在医学图像异常检测方面表现出优异的性能，但是受到显著的推断时间的影响，并且只能用于一维(1D)图像。

但是最令人兴奋的新型生成模型之一是扩散模型。

## 什么是扩散模型？

扩散模型源于[概率似然估计方法](https://towardsdatascience.com/diffusion-models-made-easy-8414298ce4da)。受气体分子从高密度到低密度区域的物理扩散(类似于“热寂”的概念)的启发，扩散模型从输入数据中收集随机噪声，同时稳步消除噪声，直到出现连贯的图像。

“扩散建模的关键概念是，如果我们可以建立一个学习模型，它可以学习由于噪音导致的信息的系统衰减，”[说](https://towardsdatascience.com/diffusion-models-made-easy-8414298ce4da) AI/ML 研究员 J. Rafid Saddiqui，“那么应该有可能逆转这个过程，因此，从噪音中恢复信息。”

OpenAI 的研究员 Lilian Weng 补充说，扩散模型“[定义了扩散步骤](https://lilianweng.github.io/posts/2021-07-11-diffusion-models/)的马尔可夫链，以将随机噪声添加到数据中，然后模型能够逆转这一过程，以创建所需的数据样本。

扩散模型——以及 VAEs 和序列到序列模型等其他生成模型——已经开始被 IBM 的人工智能研究人员[用于其开源科学发现生成工具包(GT4SD)。目标是解决诸如材料设计和发现的应用，以发现新的分子、药物和其他材料。](https://research.ibm.com/blog/generative-models-toolkit-for-scientific-discovery)

扩散模型[在半监督、全监督和强化学习应用中被证明是有效的](https://en.wikipedia.org/wiki/Generative_adversarial_network)。

## 扩散模型如何帮助检测脑瘤？

虽然扩散模型尚未用于临床设置，但最近的几项研究已经证明了它们在促进无监督的大脑异常检测和消除对医学图像手动标记的需要方面的有效性。

*   [Pinaya 等人。(2022)](https://arxiv.org/abs/2206.03461) :研究人员在病理病变的二维(2D) CT 和 MRI 图像上部署了扩散模型。实验表明，与推断时间显著减少的自回归模型相比，扩散模型可以实现“竞争性能”。据研究人员称，实验证明扩散模型在临床上是可行的。
*   [沃莱博等人。(2022)](https://arxiv.org/abs/2203.04306) :研究人员提出了一种新的弱监督异常检测方法，以“去噪”扩散模型为基础，并在 BRATS2020 脑肿瘤检测数据集和 CheXpert 胸腔积液数据集上进行评估。作者表示，他们的方法“无需复杂的训练程序就能生成非常详细的异常图。

行业专家还认为扩散和其他生成性人工智能模型是工业和机器人产品开发、下一代内容创作(如不可替代的令牌，或 NFT)和其他诊断应用的绝佳候选。

[今天就联系我们](https://www.capestart.com/about-us/capestart-is-your-end-to-end-data-annotation-machine-learning-and-software-development-partner/)与 CapeStart 的一位机器学习专家进行技术讨论，讨论各种类型的生成式人工智能模型如何帮助扩展您的医疗或科研团队。