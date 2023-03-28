# 使用 PyTorch 的深度卷积生成对抗网络

> 原文：<https://medium.com/geekculture/deep-convolutional-generative-adversarial-network-using-pytorch-ece1260acc47?source=collection_archive---------11----------------------->

## 了解如何使用 PyTorch 通过 DCGAN 生成 MNIST 图像

这篇文章将学习在 MNIST 数据集上使用 PyTorch 创建一个 DCGAN。

## 先决条件

[对 CNN 的基本了解](/datadriveninvestor/convolutional-neural-network-cnn-simplified-ecafd4ee52c5)

[使用 CNN 的示例实现](/datadriveninvestor/implementing-convolutional-neural-network-using-tensorflow-for-fashion-mnist-caa99e423371)

[了解深度卷积 GAN](/swlh/dcgan-deep-convolutional-generative-adversarial-network-1a2e55c35133)

**GANs 是由 Ian Goodfellow 在 2014 年发明的，并在论文** [**创成式**](https://papers.nips.cc/paper/5423-generative-adversarial-nets.pdf) 中首次描述…