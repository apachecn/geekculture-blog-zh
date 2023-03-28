# 现代深度学习实践者的 6 个机器学习想法

> 原文：<https://medium.com/geekculture/6-machine-learning-ideas-for-the-modern-deep-learning-practioner-f98f684db34f?source=collection_archive---------1----------------------->

## 这些协议将继续定义未来。确保你掌握了它们

为了帮助我了解您[请填写此调查(匿名)](https://forms.gle/7MfQmKhEhyBTMDUD7)

机器学习以疯狂的速度发展。有大量新的想法和程序被发明出来。就在两年前，多模态训练(用来自文本、图像、声音和视频的输入来训练 ML 模型)还是一个非常小众的想法。现在来自[谷歌人工智能](https://machine-learning-made-simple.medium.com/google-ai-sparks-a-revolution-in-machine-learning-403f4dbf3e70)和 [Deepmind](https://www.deepmind.com/blog/tackling-multiple-tasks-with-a-single-visual-language-model) 的不同出版物让每个人都有了这个想法。

当我通过大量的研究、技术出版物和与专家的互动，我发现有许多反复出现的想法，在正在开发的高价值解决方案中出现了很多。在这篇文章中，我将与您分享这些想法，这样您就可以用 ML 中一些最有影响力/最强大的想法来武装自己。

# 1:编码器-解码器对

当我们讨论惊人的机器学习解决方案时，这是经常被忽略的想法之一。从表面上看，这是一个很容易理解的概念。编码器接收你的输入，并将其编码到一个潜在的空间。解码器从潜在空间中提取矢量，并将其转换回来。这使得他们天生适合语言处理任务，他们在这方面取得了巨大的成功。

当你阅读大规模的 ML 解决方案时，如脸书的语言代码转换器、语言翻译、文本重建和大型语言模型，我们会发现处理中使用的编码器-解码器对。然而，它们的效用并不止于此。

![](img/13abba4de99e483b4ad3af0e59a3da7d.png)

Encoder-Decoder pairs have a lot of use in translation, reconstruction, deepfakes, and a bunch of other cool ideas. [Image Source](https://showmecyber.com/deepfakes-or-ai-generated-synthetic-media-not-just-for-porn-anymore/)

这在计算机视觉中有多种用途。对抗性学习、重建、图像存储和生成是一些显著的例子。它在 DALL-E 中也起着至关重要的作用。我们将文本输入编码到潜在空间中。然后我们可以用一个解码器把潜在向量解码成图像。这就是我们如何能够从文本描述中生成图像。脸书·艾的[制作场景:基于场景的文本到图像的生成，带有人类先验知识](https://arxiv.org/abs/2203.13131)。

![](img/db3591e4c323b3a31f1a6bc683857acc.png)

Taken from Make-A-Scene. The quality of images that Meta’s AI generates is stunning. [Read about how this ties into their MetaVerse aspiration](/geekculture/machine-learning-for-the-metaverse-why-metas-ai-lab-is-so-random-42975ab28a26)

它们的有效性归结于它们的简单性。获取输入向量并在潜在空间中表示它们背后的概念是一个看似强大的概念。它可以用来联系不同领域的相关概念，赋予它们无与伦比的通用性。前述来自 Deepmind 和谷歌人工智能的出版物暗示了多模态训练可能是 AGI 的关键。如果是这种情况，编码器-解码器对将发挥重要作用。我将更详细地介绍这一点，所以请确保您使用本文末尾的链接与我联系。

# 2:注意

随着大型语言模型和变形金刚的出现，我们现在可以说注意力机制已经发生了革命性的变化。变形金刚的注意力机制允许他们识别句子中重要的部分。注意力让变形金刚过滤掉噪音，捕捉相距甚远的单词之间的关系。

![](img/2c9ed0b733a263c3eeceedce3357e702.png)

Taken from the legendary Google Paper, [Attention Is All You Need](https://arxiv.org/abs/1706.03762). Not needing sequence is a big deal.

在 NLP 的上下文中，大家已经知道了这一点。我不知道的是，这甚至适用于 CV。注意力机制允许变形金刚保持“图像的全局视图”,允许它们提取与 ConvNets 非常不同的特征。记住，CNN 使用核来提取特征，这意味着它们只能找到局部特征。注意让变压器绕过这个。

![](img/c8bdc6e752ed48415b17f3c161a55b7e.png)

Since Transformers also use Conv Priors, this is a best of both worlds sort of deal.

上图取自非常有趣的，[视觉变形金刚看起来像卷积神经网络吗？](https://arxiv.org/abs/2108.08810)有趣的是，我稍后会对这篇论文进行分解。重要的方面是下面的引用，也来自该文件。

> *…证明了获取更多的全球信息也会导致数量上不同的特征，而不是由 ResNet 下层的局部感受野计算出来的特征*

# 3:随机森林

我知道你们中的一些人看到这里的内容会很困惑。随机森林是经典的机器学习模型。它们和逻辑回归、朴素贝叶斯和 KNNs 一样是你的第一个模型。那么为什么它们会出现在现代技术的列表中呢？

![](img/126efe5e191a651a5e204f4795cbc8ee.png)

For more Machine Learning wisdom, follow my Twitter. Your ML will be 10x’d

RF 分类器和回归器是一样的。没错。但是射频的想法已经发展成各种各样的其他技术。随机森林可有效用于以下任务:

1.  离群点检测
2.  特征工程
3.  特征重要性、缩减取样和选择
4.  数据插补

RFs 是惊人的，因为他们的多功能性使他们强大。它们对异常值具有鲁棒性，可以处理缺失值，非常适合杂乱的数据集。在我的文章[如何处理缺失的环境数据](/codex/how-to-handle-missing-environmental-data-ff0354ec013b)中，我展示了这些属性如何完美地处理各种数据集。

# 4:随机性

如果你是我最初的追随者之一，你会知道我是你的机器学习训练中引入噪音和随机性的超级粉丝。我已经强调了一段时间了，那时候它还不是主流。这些天，所有酷小孩都在这么做。

![](img/753a3811330deae14e4033031ddc2744.png)

From Microsoft’s “[Efficiently and effectively scaling up language model pretraining for best language representation model on GLUE and SuperGLUE](https://www.microsoft.com/en-us/research/blog/efficiently-and-effectively-scaling-up-language-model-pretraining-for-best-language-representation-model-on-glue-and-superglue/)”. For my analysis of this publication, [check out this link](/geekculture/microsoft-timnit-gebru-and-google-ai-85384edc6af7)

[当你想创建可以概括许多分布的模型时，随机性可以改变游戏规则](https://medium.datadriveninvestor.com/why-and-how-to-integrate-randomness-into-your-machine-learning-projects-53c1c30561e)。伴随着这种嘈杂的训练，对对抗性训练有潜在的好处。本文深入探讨[在深度学习](/mlearning-ai/using-randomness-effectively-in-deep-learning-910c60adc067)中有效利用随机性。

# 5:卷积神经网络

这是一个经典，但在这里提到它是有原因的。CNN 简单、直观，并且可以有很好的性能。虽然他们专门从事计算机视觉任务，但他们是该领域的王者。我不需要坐在这里谈论 CNN 有多棒。那会浪费每个人的时间。

![](img/fc289c150c2410134dd2e273c74cff93.png)

CNNs use the sliding window technique to build their feature maps. As you can see, Good Machine Learning requires good software engineering. [Image Source](https://towardsdatascience.com/a-comprehensive-guide-to-convolutional-neural-networks-the-eli5-way-3bd2b1164a53)

当我们讨论 ConvNets 时，经常忽略的是它们使用内核自动提取特征的方式。我们知道[特征将决定一个模特的表现](/mlearning-ai/researchers-discover-possible-reason-why-adversarial-perturbation-works-f65e64d9eb7)。随着超大规模机器学习变得更加普遍，良好的预处理的重要性将成为一个区分因素。掌握 CNN 的特征提取方法将会很好地为你服务。

# 6:甘斯

GAN 哲学最近有了一点复苏。更准确地说，我们已经看到一些改变游戏规则的技术实现了 GAN 架构。这让这个想法又回到了“流行”话语中。通过让两个目标相反的模型相互对抗来训练它们的想法会产生惊人的结果。

这远远超出了传统的 GANs。gan 是基于鉴别性学习者和生成性学习者的结合。我们可以以多种方式将这两种技术结合起来，从而产生一些不同寻常的结果。我们可以在这些情况下实现进化学习器，这将允许你获得一些特殊的结果，例如与[项目日内瓦](/geekculture/geneva-state-censorship-beat-by-genetic-algorithms-b95deff5cc52)。

本文到此为止。自然，掌握机器学习对于真正利用这些强大的想法至关重要。[这篇文章给你一个循序渐进的计划](/geekculture/how-to-learn-machine-learning-in-2022-9ef2ea904986)，利用免费资源培养机器学习的熟练程度。与其他训练营/课程不同，该计划将帮助你发展基本技能，并为你在该领域的长期成功做好准备。

![](img/f6980febd04f6f9e31b3f2bfc3c40e67.png)

Machine Learning is complex because of the many moving parts. Constantly learning is the best way to thrive in that domain.

对于机器学习来说，软件工程、数学和计算机科学的基础至关重要。它将帮助你概念化，建立和优化你的 ML。我的每日时事通讯，[编码采访变得简单](https://codinginterviewsmadesimple.substack.com/)涵盖了算法设计、数学、最近的科技事件、软件工程等主题，让你成为更好的开发人员。 [**我目前正在进行一整年的八折优惠，一定要去看看。**](https://codinginterviewsmadesimple.substack.com/subscribe?coupon=1e0532f2)

![](img/5957be47d77ccfbcaa28efa3c992aad9.png)

我创建了[编码面试，使用通过指导多人进入顶级科技公司而发现的新技术使面试变得简单](https://codinginterviewsmadesimple.substack.com/p/faqs-and-about-this-newsletter?r=4tnbw&s=w&utm_campaign=post&utm_medium=web)。时事通讯旨在帮助你成功，避免你在 Leetcode 上浪费时间。我有一个 100%满意的政策，所以你可以尝试一下，没有任何风险。[您可以阅读常见问题解答，并在此了解更多信息](https://codinginterviewsmadesimple.substack.com/p/faqs-and-about-this-newsletter?r=4tnbw&s=w&utm_campaign=post&utm_medium=web)

![](img/548a96a2f2322893319803ec7e15fa2b.png)

如果你也有任何有趣的工作/项目/想法给我，请随时联系我。总是很乐意听你说完。

以下是我的 Venmo 和 Paypal 对我工作的金钱支持。任何数额都值得赞赏，并有很大帮助。捐赠解锁独家内容，如论文分析、特殊代码、咨询和特定辅导:

https://account.venmo.com/u/FNU-Devansh

贝宝:【paypal.me/ISeeThings 

# 向我伸出手

使用下面的链接查看我的其他内容，了解更多关于辅导的信息，或者只是打个招呼。另外，查看免费的罗宾汉推荐链接。我们都得到一个免费的股票(你不用放任何钱)，对你没有任何风险。**所以不使用它只是失去免费的钱。**

查看我在 Medium 上的其他文章。:[https://rb.gy/zn1aiu](https://rb.gy/oaojch)

我的 YouTube:[https://rb.gy/88iwdd](https://rb.gy/88iwdd)

在 LinkedIn 上联系我。我们来连线:[https://rb.gy/m5ok2y](https://rb.gy/f7ltuj)

我的 insta gram:[https://rb.gy/gmvuy9](https://rb.gy/gmvuy9)

我的推特:[https://twitter.com/Machine01776819](https://twitter.com/Machine01776819)

如果你正在准备编码/技术面试:[https://codinginterviewsmadesimple.substack.com/](https://codinginterviewsmadesimple.substack.com/)

获得罗宾汉的免费股票:[https://join.robinhood.com/fnud75](https://join.robinhood.com/fnud75/)