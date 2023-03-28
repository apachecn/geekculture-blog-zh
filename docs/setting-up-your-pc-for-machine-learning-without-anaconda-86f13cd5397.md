# 在没有 anaconda 的情况下设置您的 PC 进行机器学习。

> 原文：<https://medium.com/geekculture/setting-up-your-pc-for-machine-learning-without-anaconda-86f13cd5397?source=collection_archive---------9----------------------->

![](img/c569f061e3d075a15e10f3a51a5a5e6e.png)

Photo by [Scott Graham](https://unsplash.com/@homajob?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/machine-learning?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

Anaconda 是一个非常方便的包，它包含了机器学习所需的大多数库，并且附带了 jupyter notebook。

当我开始设置我的工作 windows 机器时，我发现由于旧的 windows 版本和一些预依赖，我在用-conda 命令安装包时遇到了问题。

在清除了堆栈溢出之后，我得到了很多这样的快速修复，比如改变路径或者升级命令，或者根本不使用 conda 命令。我发现我的机器已经预装了很多库。所以基本上 anaconda 增加了很多开销，或者更糟，各种现有版本和路径变得复杂，机器难以处理。

如果你厌倦了安装，想尽快开始真正的工作，这是为你准备的。

首先检查 python 是否安装在您的设备上以及版本。我建议至少安装 3.5 以上的版本，并正确设置路径。你可以找到很多这方面的在线教程。因为所有系统通常都有 python，所以在本教程中我将跳过它。

在终端中使用此命令检查是否安装了 pip

```
pip help or pip3 help 
```

如果你得到一个像命令不能被识别的结果。使用以下命令安装它。

```
python get-pip.py
```

如果您安装了 pip，请升级 pip

```
pip install --upgrade pip
```

正确安装 pip 后，打开一个编辑器，导入您的机器学习项目所需的库

但是在此之前，如果你打算使用 Pytorch 或 tensorflow，我建议你现在就按照官方文档安装它。他们也使用 pip。别担心！

现在检查以下哪些模块没有安装

```
import numpy
import pandas
import matplotlib 
import sklearn //Scikit-learn
import cv //opencv //image processing library
```

我发现在 python 和 pip 更新之后，大部分基本库已经在那里了。检查哪些是导致模块未找到错误的原因，并相应地安装它们。

```
pip install numpy
pip install pandas
pip install opencv-python
pip install matplotlib
pip install scikit-learn 
pip install scikit-image
```

请记住 ML 有很多库，但好消息是无论何时你使用它们，你都需要先导入它们，如果有模块未找到错误，你可以立即安装库。

下一个编辑器你可以安装 vscode/ sublime text 或者 jupyter notebook。你不需要在任何单独的环境中处理 ML 项目，这将节省你的机器上的一点空间。