# 在 PyTorch 中训练一个用于医学诊断的有效网络模型

> 原文：<https://medium.com/geekculture/train-a-pytorch-efficientnet-model-for-medical-diagnosis-1d000c540bd?source=collection_archive---------1----------------------->

随着全球新冠肺炎病例的激增，快速准确的肺炎医学诊断比以往任何时候都更加重要。目前的计算机视觉和深度学习技术已经使这成为可能。

在这篇博文中，我们将应用 PyTorch Image Models (timm)中可用的 EfficientNet 模型来识别测试集中的肺炎病例。

让我们来看一下最终结果(右边的蓝条是计算出的概率):