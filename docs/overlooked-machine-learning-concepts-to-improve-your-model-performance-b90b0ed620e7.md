# 被忽视的机器学习概念来提高模型性能

> 原文：<https://medium.com/geekculture/overlooked-machine-learning-concepts-to-improve-your-model-performance-b90b0ed620e7?source=collection_archive---------1----------------------->

## 这些都是强大的想法，会让你的机器学习管道更上一层楼。

为了帮助我了解您[请填写此调查(匿名)](https://forms.gle/7MfQmKhEhyBTMDUD7)

感谢互联网和开源社区，现在是开始构建利用人工智能和数据为各种组织创造有价值的见解的产品的最佳时机。在这篇文章中，我将分享一些惊人的技术，这些技术将使您的机器学习解决方案变得更好。这些技术已经在项目中取得了很多成功，但太多的 ML 从业者超越了这些，直接跳到昂贵的深度学习方法。

# 随机选择

对于那些已经关注我的工作一段时间的人来说，这不会让你感到惊讶。从好处来看，在你的机器学习管道中实现一定程度的随机性将改善你的整体网络——在性能、健壮性甚至成本方面。例如，[谷歌的研究人员能够击败 SOTA 的图像分类模型**，同时使用少 12 倍的图像(并且不需要标记这些图像)**](https://medium.datadriveninvestor.com/the-next-big-thing-in-image-classification-self-training-with-noisy-student-for-improving-22d52dc74dda) **。**

> 我们展示了一个与直觉相反的结果，即在不完美的估计量上增加更多的变异源可以更好地接近理想的估计量，而计算成本却降低了 51 倍。
> 
> 从论文来看，“机器学习基准中的 [**方差核算**](https://arxiv.org/abs/2103.03098) ”。[在这里阅读我对那篇论文的分析](/mlearning-ai/why-you-need-to-spend-more-time-evaluating-your-machine-learning-models-e1e3258fe7d)

为什么会这样？最可能的解释是，在你的训练协议中加入随机和混乱的元素，实际上扩大了你的模型搜索的解空间。因此，最终的模型配置将更适合于推广到更大可能的输入集。**这在构建解决方案时很有帮助，这些解决方案可以推广到您将在现实世界中看到的通常混乱无序的输入。**

令人惊奇的是，添加到数据集的随机性并不需要太花哨。在计算机视觉领域， [TrivialAugment](https://youtu.be/DzdVsME6NOM) 和 [RandAugment](https://youtu.be/tt-JpNIUmKg) 是最成功的数据增强协议。这两种协议都非常简单，并且比那些花哨的协议要好得多。精心制作的简单策略会产生大量的输入噪声(从而产生多样性)，并且不会像更复杂的策略那样引入太多的偏差，更复杂的策略采用整个数据集，因此通常会模仿原始数据集的分布。

![](img/43a25a9b9f990d4e0bd7c7f5d0dfd361.png)

Applying simple policies to make the input images noisy can work wonders.

比较深度学习模型的研究进一步支持了我的假设，即更多的噪音→更大的搜索空间→更好的结果。下面是我的文章，分解了一篇比较深度学习集成和贝叶斯网络的论文。作者表明，集成优于 bnn，因为它们能够通过更多样化的搜索空间进行采样。

[](/swlh/why-deep-learning-ensembles-outperform-bayesian-neural-networks-dba2cd34da24) [## 为什么深度学习集成优于贝叶斯神经网络

### 他们不做同样的事情吗？？

medium.com](/swlh/why-deep-learning-ensembles-outperform-bayesian-neural-networks-dba2cd34da24) 

如果你对在你的管道中工作随机性感兴趣，看看我关于在深度学习中有效使用随机性的指南。它将涵盖你可以实现随机性的地方和其他重要的细节。如果你需要更多的帮助，你可以随时联系我。联系信息在本文末尾。

# 进化方法

说到可以遍历非常多样化的搜索空间的东西，是时候涵盖机器学习工程师武器库中可能最容易被忽视的工具了。EAs 上的不尊重程度令人难以置信。我交谈过的从未考虑过进化方法的曼梯里人的数量是惊人的。是时候改变这种状况了。

进化算法令人惊叹的原因有很多——它们便宜，可以处理比神经网络更多样化的问题，它们表现出令人惊叹的性能，并且它们的训练可以很容易地被跟踪和分析。我们已经看到越来越多的人在更高层次的优化问题中使用进化算法作为一层。它们被用于一些最酷的项目中，包括对抗政府审查。上面的主题包含了更多的信息，包括 EAs 在神经结构搜索、计算机视觉等领域的各种实现。

# 贝叶斯学习

你可以根据他们对贝叶斯方法的熟悉程度来区分优秀的 ML 工程师和伟大的 ML 工程师。机器学习领域的大多数人不理解数学，因此对贝叶斯统计和因果推理并不完全适应(甚至意识不到)。很多“酷”的 ML 研究并没有涉及这个话题，但是贝叶斯学习已经改变了我在 ML 中的效率。

[](https://medium.datadriveninvestor.com/math-is-a-language-this-is-how-you-should-learn-it-cbc7aac45a8b) [## 数学是一种语言。这是你应该如何学习它。

### 如何自学数学中的难题，为自己在人工智能、技术和工程领域的职业生涯打下坚实的基础

medium.datadriveninvestor.com](https://medium.datadriveninvestor.com/math-is-a-language-this-is-how-you-should-learn-it-cbc7aac45a8b) 

贝叶斯学习可能是一件棘手的事情来学习和理解，但它将是一个伟大的除了你的军火库。我只是通过在 ForeOptics 从事供应链分析工作才真正掌握了它(感谢我的经理 Shaheen 在这方面的所有帮助)，但从那以后，我已经能够解决一些非常有趣的问题。为了发展你的技能，使用贝叶斯学习搜索一些演讲/论文，以了解它被应用的一些背景(以及为什么它在那里工作)。利用这一点来获得对主题的欣赏。然后(也只有到那时)研究这个主题的数学和编码。如果你在理解数学上有困难，阅读上面的文章可以帮助你。

![](img/0934e3dc805ae5f1b0a6575aec1a2ca0.png)

[Source](https://csinva.io/notes/research_ovws/ovw_causal_inference.html). Check it out. It’s amazing

如果你想进入 ML，t [这篇文章给你一个逐步发展机器学习能力的计划](/geekculture/how-to-learn-machine-learning-in-2022-9ef2ea904986)。它使用免费资源。与其他训练营/课程不同，这个计划将帮助你发展基本技能，并为你在该领域的长期成功做好准备**。**

![](img/b68f588e9ce7dfcad1e152af02a2eb49.png)

对于机器学习来说，软件工程、数学和计算机科学的基础至关重要。它将帮助你概念化，建立和优化你的 ML。我的每日时事通讯，[Technology interview simpled](https://codinginterviewsmadesimple.substack.com/)涵盖了算法设计、数学、最近的科技事件、软件工程等主题，让你成为更好的开发人员。 [**我目前正在进行一整年的八折优惠，一定要去看看。**](https://codinginterviewsmadesimple.substack.com/subscribe?coupon=1e0532f2)

![](img/54ec0ec6ad9206f4a78071c29e9ddb95.png)

我创造了[技术面试，利用通过指导多人进入顶级科技公司而发现的新技术，使面试变得简单](https://codinginterviewsmadesimple.substack.com/p/faqs-and-about-this-newsletter?r=4tnbw&s=w&utm_campaign=post&utm_medium=web)。时事通讯旨在帮助你成功，避免你在 Leetcode 上浪费时间。我有一个 100%满意的政策，所以你可以尝试一下，没有任何风险。[你可以在这里阅读常见问题并了解更多信息](https://codinginterviewsmadesimple.substack.com/p/faqs-and-about-this-newsletter?r=4tnbw&s=w&utm_campaign=post&utm_medium=web)

![](img/0c0a9791c39039f80038e5df857367f1.png)

如果你也有任何有趣的工作/项目/想法给我，请随时联系我。总是很乐意听你说完。

# 向我伸出手

使用下面的链接查看我的其他内容，了解更多关于辅导的信息，或者只是打个招呼。另外，查看免费的罗宾汉推荐链接。我们都得到一个免费的股票(你不用放任何钱)，对你没有任何风险。**所以不使用它只是失去免费的钱。**

查看我在 Medium 上的其他文章。:[https://rb.gy/zn1aiu](https://rb.gy/oaojch)

我的 YouTube:[https://rb.gy/88iwdd](https://rb.gy/88iwdd)

在 LinkedIn 上联系我。我们来连线:[https://rb.gy/m5ok2y](https://rb.gy/f7ltuj)

我的 insta gram:[https://rb.gy/gmvuy9](https://rb.gy/gmvuy9)

我的推特:[https://twitter.com/Machine01776819](https://twitter.com/Machine01776819)

如果你正在准备编码/技术面试:[https://codinginterviewsmadesimple.substack.com/](https://codinginterviewsmadesimple.substack.com/)

获得罗宾汉的免费股票:[https://join.robinhood.com/fnud75](https://join.robinhood.com/fnud75/)