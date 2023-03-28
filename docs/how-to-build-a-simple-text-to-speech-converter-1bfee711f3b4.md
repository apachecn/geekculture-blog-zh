# 如何搭建一个简单的文字语音转换器？

> 原文：<https://medium.com/geekculture/how-to-build-a-simple-text-to-speech-converter-1bfee711f3b4?source=collection_archive---------7----------------------->

## 用 Python 构建文本到语音转换器指南

![](img/29ed0cf6cc1cb32f0b4c1e94fa2a8dcc.png)

Photo by [Volodymyr Hryshchenko](https://unsplash.com/@lunarts?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/text-to-speech?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

今天早上，我注意到了 Medium 上的文章的“听”功能，并立即喜欢上了它。我相信这对于移动学习来说是一个很好的补充，这让我思考，我如何自己构建一个？

这是一个非常简单的实现(*猜猜朗读一篇文章需要多少行？因此，如果你正在寻找一个快速的项目或者只是想做一些有趣的东西，那么这篇文章就是为你准备的:)*

让我们开始吧。

# 先决条件

设置 Python 环境，如果您已经设置了 Python3 环境，那么您可以跳过这一部分，转到步骤 1。

**第一步:安装 Python、pip 和 virtualenv**

互联网上有很多关于如何安装 Python 的指南，我指的是 Python 3。如果你还在玩 Python 2，我有一个词送给你——*为什么？！*

这里有一个这样的指南

**第 0 步:创建一个新的虚拟机**

我个人喜欢为特定的东西创建新的虚拟，否则它只会搞乱我的系统。不要问我以前不使用 virtualenv 时，我花了多少时间来修复冲突的依赖关系，所以向我学习，创建一个新的。让我们称我们的项目为 tinytalker，并为其创建一个环境:

```
python3 -m venv thetinytalker
source thetinytalker/bin/activate
```

嗯，就是这样。现在 Python 环境已经设置好了，让我们继续有趣的事情。

# **逐步指南**

**步骤 1:安装必要的依赖项**

现在，你只需要两个库来让你的文本说话或者构建*“the tiny talker”:*

*   谷歌文本语音库: *gtts*
*   Playsound: *playsound，PyObjC*

让我们安装

```
pip install gtts
pip install playsound
pip install PyObjC
```

请耐心等待，安装工作可能需要几分钟，在此之前喝杯咖啡吧。

**第二步:** **让你的文字说话**

在您选择的任何编辑器中将下面的代码保存为 tinytalker.py，并使用

```
python3 thetinytalker.py
```

或者，您也可以在 Python shell 中运行它。

嗯，就这样吧！同样，如果你认识到《办公室》中的这句话，给我大声喊出来:P

就这么简单。因此，用不到十行代码，你就可以让你的文本说话。

希望你读得开心，学到了一些东西。如果您有任何问题或反馈，请留下评论或在 LinkedIn 上与我联系，说声“嗨”！

快乐编码。
JD