# 我的网络如何达到 99%的准确率

> 原文：<https://medium.com/geekculture/how-my-lenet-achieves-99-accuracy-acb8a1c737f0?source=collection_archive---------16----------------------->

![](img/8742fc395a2db3bb83db5183330f514c.png)

Photo by [Ian Stauffer](https://unsplash.com/@ianstauffer?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

微调在模型训练中的作用很大，认识到每个超参数的意义让你成功。

在这篇文章中，我将向您展示我如何通过微调三个超参数，在 MNIST 手写数字识别上达到 99%的最高准确率。我还尝试从头开始实现一个经典的 CNN 模型 LeNet-5，让我熟悉 CNN 模型的结构和基本组件。更重要的是，我将在 [Flux.jl](https://fluxml.ai/) 中构建我的 LeNet-5 模型，以展示一个 Julia 神经网络框架的例子。

# 基础模型

> *你可以从* [*我的回购*](https://github.com/Cuda-Chen/flux-lenet) *获取代码。*

基础模型是大家熟知的 5 层 LeNet [ ]，实现采用自 Flux.jl 模型 zoo [ ]。因此，原始 LeNet 和 Flux.jl 模型 zoo [ ]中的实现之间存在一些差异:

1.  LeNet 中卷积层的激活函数使用 **sigmoid** ，而在 Flux.jl model zoo 中使用 **ReLU** 。
2.  LeNet 中的池层使用**平均**池，而在 Flux.jl 模式中 zoo 使用**最大**池。
3.  LeNet 中的 pooling 层激活函数使用**比例双曲正切**，而 Flux.jl model zoo 中的使用**恒等式**(线性)。
4.  LeNet 原始论文中使用的多类分类使用**欧氏径向基(RBF)函数**。[3]但是，在 Flux.jl 的实现中使用了 **softmax** 。

为了方便起见，我列出了我的实现的结构:

*The structure of base model. You can click “view raw” to show the image in new tab. Sorry for your inconvenience because NN-SVG (http://alexlenail.me/NN-SVG/LeNet.html) does not have any options to resize the image.*

# 让我们微调一下！

因此，hypermeter 调整在机器学习模型开发中起着至关重要的作用。虽然 Flux.jl 的 LeNet 实现可以达到 98%的 top-1 准确率，但我还是想试试能否突破极限。更重要的是，通过实验微调，我可以获得哪些参数在某些任务中起主要作用的知识。

# 基线及其性能

> *基线模型可以在这里找到:*[*https://github . com/flux ml/model-zoo/blob/master/vision/conv _ mnist/conv _ mnist . JL*](https://github.com/FluxML/model-zoo/blob/master/vision/conv_mnist/conv_mnist.jl)

# 批量

批量意味着在一次迭代中使用多少训练样本。此外，它表示您在摄取一定数量的训练样本后，更新或正式计算损失，然后反向传播模型的参数。因此，假设以下场景:

1.  如果您在摄取**整个数据**后更新参数。你可能得到一个快速的参数更新时间，但是模型在实际情况下表现不佳，因为模型陷入局部极小值的陷阱。此外，它需要大量的内存来加载数据。
2.  如果你在摄取**后更新了参数的每个数据**(每次迭代中只有一个数据)。您可能会得到一个具有奇妙结果的模型，但是它需要花费大量的时间来训练，因为它会在每次迭代中更新参数。

因此，选择正确的批量数量可以:

*   减少训练时间和记忆
*   覆盖更好的性能

在这篇文章中，我尝试了不同数量的批量，我的培训平台的最佳批量是 **32** 。

以下段落是不同批量的训练日志:

## 32

## 64

## 256

## 512

## 正则参数

正则化是添加惩罚，以便模型降低变得过拟合的概率。通常，我们可以使用 L1 和 L2 正则，我选择 L2 正则为我的 LeNet-5 模型。

在本实验中，最佳 L2 正则化参数为 **1e-6** 。

像往常一样，我把训练日志与不同的正则化参数:

## 1e-2

## 1e-4

## 1e-6

# 【计算机】优化程序

机器学习中的优化器是根据预先指定的参数来改变学习率，从而改变模型的学习率，使模型更容易推广。在这篇文章中，我在常见的 ADAM 中选择了三个优化器:ADAMW、NADAM 和 AdaBelief。关于这些优化器的描述，可以访问[flux . JL](https://fluxml.ai/Flux.jl/stable/training/optimisers/#Optimisers)的优化器文档。

在这篇文章中，最好的优化器是 ADAMW。

这里是不同优化器的训练日志:

## 阿达姆

## 那达慕

## 阿达信仰

# 结论

在这篇文章中，我构建了经典的 LeNet-5 模型，不仅练习了我的机器学习技能，还让我熟悉了新兴的 Flux.jl 框架。我还展示了三个可能的标准——批量大小、正则化和优化——用于超参数调优或微调的过程。最后，我给大家展示了我的 LeNet-5 模型在 MNIST 数据集上可以达到 99%的 top-1 准确率。

# 显示培训环境的列表

*   CPU:英特尔酷睿 i7–9700 CPU，3.00GHz
*   内存:16 磅
*   操作系统:Fedora 33 (Linux 内核 5.13.12)
*   朱莉娅版本:1.6.2
*   Flux.jl 版本:v0.12.4

# 参考

【[http://yann.lecun.com/exdb/publis/pdf/lecun-98.pdf](http://yann.lecun.com/exdb/publis/pdf/lecun-98.pdf)

[][https://github . com/flux ml/model-zoo/blob/33 F5 C5 c 472 c 321 a 50 fc 2105358 a 00 EB 7 B3 EC 0 FFA 5 e/vision/conv _ mni ST/conv _ mni ST . JL # L21](https://github.com/FluxML/model-zoo/blob/33f5c472c321a50fc2105358a00eb7b3ec0ffa5e/vision/conv_mnist/conv_mnist.jl#L21)

https://pabloinsente.github.io/the-convolutional-network

*原载于 2021 年 9 月 23 日*[*https://cuda-Chen . github . io*](https://cuda-chen.github.io/deep%20learning/2021/09/23/lenet-99.html)*。*