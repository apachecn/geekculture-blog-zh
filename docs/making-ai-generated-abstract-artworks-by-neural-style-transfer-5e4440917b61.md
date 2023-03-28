# 通过神经风格转移制作人工智能生成的抽象艺术品

> 原文：<https://medium.com/geekculture/making-ai-generated-abstract-artworks-by-neural-style-transfer-5e4440917b61?source=collection_archive---------3----------------------->

最近在研究最有意思的深度学习课题，[生成对抗网络](https://en.wikipedia.org/wiki/Generative_adversarial_network) (GAN)。GAN 是一个[无监督机器学习](https://en.wikipedia.org/wiki/Unsupervised_learning) ( [生成](https://en.wikipedia.org/wiki/Generative_model))模型，它自动发现输入数据的模式，并生成与输入数据相似的输出数据。

大多数人认为 GAN 是为了好玩而做的(比如 [Deepfake](https://en.wikipedia.org/wiki/Deepfake) 、[wave GAN 合成音频](https://towardsdatascience.com/synthesizing-audio-with-generative-adversarial-networks-8e0308184edd)等)。).GAN 有很多实际的使用案例。例如，用于训练图像数据集的数据扩充、文本生成、文本到图像生成、超分辨率、图像…