# 亚马逊如何让机器学习值得信赖

> 原文：<https://medium.com/geekculture/how-amazon-makes-machine-learning-trustworthy-92296297a541?source=collection_archive---------4----------------------->

## 在 ChatGPT 和机器学习中围绕偏见的所有讨论中，这些技术可能非常有帮助

机器学习最近风靡全球。由于所有这些惊人的结果，公司纷纷将数据驱动的决策纳入他们的流程。鉴于 DALLE、StableDiffusion 和现在的 ChatGPT 的所有令人惊叹的演示，越来越多的人开始意识到人工智能的潜力。然而，一些人已经开始担心这些模型的潜在危害。最近，ChatGPT 获得了一些关注，因为用户发现它可以生成一些*辣*的输出。看看 ChatGPT 如何根据种族和性别来识别优秀的科学家。

![](img/2996cbd77d861fef2b9ad83c0a9bbdf7.png)

For a non-clickbait, non-inflammatory analysis of this output and why this matters, [check out this post](https://www.linkedin.com/posts/devansh-devansh-516004168_a-scientist-recently-used-chatgpt-to-create-activity-7005597022409957376-1AAn/). Too many commentators have been focusing on the wrong things.

数据科学和深度学习中的偏见并不是什么新鲜事。大型模型对偏见和复制数据集中的偏见编码的敏感性也是如此。我已经说了 2 年了。但是理解如何处理这个问题现在比以往任何时候都重要。幸运的是，科技巨头亚马逊在这一领域取得了长足的进步。在这篇文章中，我将分解他们的出版物- [*在 Alexa AI*](https://www.amazon.science/blog/advances-in-trustworthy-machine-learning-at-alexa-ai) *-* 上的可信机器学习进展，在那里他们分享了一些他们用来创建更安全、更公平的 AI 的技术。

![](img/0c1ad108c61c7760d78b216051d37428.png)

One example of trying to get rid of bias in the dataset. Amazon’s Model ignores the gendered language in the prompt. FROM “[MITIGATING GENDER BIAS IN DISTILLED LANGUAGE MODELS VIA COUNTERFACTUAL ROLE REVERSAL](https://www.amazon.science/publications/mitigating-gender-bias-in-distilled-language-models-via-counterfactual-role-reversal)”

如果您对创建更好的 ML 管道感兴趣，那么这不是您想要错过的话题。让我们开始吧。

# 技巧 1:隐私保护 ML

为了理解这一点以及为什么这很重要，让我们首先了解一个关于机器学习的基本事实。ML 模型接受一些输入并生成一些输出。生成的输出取决于模型在训练阶段发现的规则。这是不言而喻的，但是规则取决于我们提供给模型的输入。对大多数人来说很明显。那我为什么要谈论它呢？

原来这带来了一些隐私风险。输出有可能被用来推断输入的细节。你的数据可能会落入你从未同意过的人手中。这就是差别隐私概念的由来。引用该出版物，“*[*差分隐私*](https://www.amazon.science/tag/differential-privacy) *(DP)背后的直觉是，对模型输出的访问不应提供任何关于使用什么输入来训练模型的提示。**

*这是怎么算出来的？" *DP 将这种直觉量化为在去除单个输入之后，在给定数据集上训练的模型的输出与在相同数据集上训练的相同模型的输出之间的差异(概率上的差异)* ***。*** “以这种方式，它让我想起了基于排列的特性的重要性，但是我们不是在特性中洗牌，而是丢弃值。这是一种量化单个样本对您的训练过程的影响的有趣方式。*

*Amazon 实现这一点的主要方法之一是使用输入噪声。[我已经广泛介绍了向深度学习管道中添加随机性的伟大之处](/mlearning-ai/using-randomness-effectively-in-deep-learning-910c60adc067)。然而，基于他们的写作，亚马逊似乎尝试了一个稍微不同的方向。他们没有使用噪声作为添加混乱的手段，而是使用噪声来隐藏参数与训练数据的关系。下图给出了一个例子。*

*![](img/a94f2895e26201f7e52577b525775710.png)*

*Taken from their publication [Improving the accuracy of privacy-preserving neural networks](https://www.amazon.science/blog/improving-the-accuracy-of-privacy-preserving-neural-networks)*

*我通常推荐的方法是为了提高管道的通用性和健壮性。显然，加入完全随机的噪音会有助于保护隐私。**但是，它会导致性能下降。**亚马逊的方法更有利于保持更高的性能。然而，请记住，要充分利用亚马逊的方法，您需要进行明智的替换。你不能用香肠代替波士顿。**寻找合适的替代品可能会增加你的计算成本**。作者承认这一点-*

> *添加 DP 机制的另一个副作用是增加了培训时间。*

*亚马逊还做了一些非常有趣的研究，研究人们如何使用各种技术重建训练样本。要了解更多关于它和它的防御对策，查看他们的出版物[自然语言理解模型中的金丝雀提取](https://www.amazon.science/publications/canary-extraction-in-natural-language-understanding-models)*

*![](img/a055b3b7efe5ef34926632c8f9178f08.png)*

*AlphaSignal 是一个免费的机器学习顶级发展每周总结。他们使用人工智能进行排名，并向您发送该领域的顶级发展。如果你正在寻找一些有助于跟上机器学习步伐的东西，请查看它们。阅读它们是与这个领域保持联系的一个很好的方式，并且支持我的写作，不需要你付出任何代价。*

*[](https://alphasignal.ai/?referrer=Devansh) [## 阿尔法信号|机器学习的极品。艾总结的。

### 留在循环中，不用花无数时间浏览下一个突破；我们的算法识别…

alphasignal.ai](https://alphasignal.ai/?referrer=Devansh)* 

# *技巧 2:联合学习*

*想想所有的 Alex 设备，Prime 视频应用程序，以及人们用于亚马逊账户的不同设备。如果亚马逊直接将数据发送回中心，他们的成本将会失控。更不用说，亚马逊数据中心的巨大隐私红旗存储着你的对话、购物等。显然，这不是一个好主意。但是，如何根据新的用户交互来更新模型呢？*

*如果您只是让模型在本地设备上更新会怎么样？比如说，有一天我在手机上看了很多 Prime 上的恐怖片。因此，我们更新了手机上的推荐系统，以适应我的新口味。一旦这些更新完成，我就与亚马逊中心分享这些更新。你，我的爱人，刚刚学会了联合学习。*

*![](img/6e49d5fff5e1c86a88121c02fbed2d36.png)*

*这有几个好处。首先，模型更新的数据比原始数据小得多，这使得处理和存储成本低得多。其次，这在隐私方面带来了巨大的好处。即使有人获得了这些数据，他们看到的也只是一堆胡言乱语。模型更新数据不是人类可读的，所以没人能看到你大吃大喝的样子。而且在不知道确切架构的情况下，它无法插入模型来重构你的习惯。*

*![](img/d22271978721070f147b5745b0c2000b.png)*

*Photo by [Mick Haupt](https://unsplash.com/@rocinante_11?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)*

# *技巧 3:ML 中的公平性*

*机器学习中一个被忽视的问题是有偏数据集的存在。当您从不能准确表示基础统计数据的数据源中采样数据时，通常会出现有偏差的数据集。例如，假设你想了解国民对一项新政策的看法。但在你的调查中，你得到的回应大多来自大学生。在这种情况下，你的分析会有偏差，因为你的国家不是大多数都是大学生。有偏见的数据集是一个比大多数人意识到的更大的问题。*

*以我在本文开头分享的种族主义者 ChatGPT 为例。大多数有头脑的 LinkedIn 影响者都乐于称之为种族歧视，并在此结束他们的分析。然而，这是不准确的。实际上，这很可能是有偏差的数据样本的情况。ChatGPT 可能搜集了主要是美国的数据集，并据此创建了它的分析。我这么说是基于这样一个事实，我的种族(印度人/东南亚人)甚至没有在基于智力的种族排名中被提及(我们被归入另一类) 。考虑到有这么多东南亚人，不把他们作为自己的种族是没有意义的。除非你考虑到这样一个事实，即在美国的数据集中，拉丁美洲人比东南亚人(或土著人等)被提及得多。这是前面提到的排名-*

*![](img/3bf8e2a838f09a68d8e9916b724ee667.png)*

*亚马逊提到了 LLM 数据集有偏差的问题*

> *自然语言处理应用程序越来越依赖于在**本质上有偏见的网络规模语料库**上训练的大型语言模型，这放大了建立更健壮模型的准确公平度量和程序的重要性。*

*他们的出版物“[减轻知识图嵌入中的社会偏见](https://www.amazon.science/blog/mitigating-social-bias-in-knowledge-graph-embeddings)”对此进行了更详细的研究。它涵盖了偏见存在的几种有趣的方式。*

*![](img/ccc6562680ced27cf5b6cbf5d031a457.png)*

*他们使用各种技术，如属性替换，来对抗偏见，否则这些偏见会被编码到知识图中。*

*![](img/edacf5fb2a3056955412cf5546c0ed33.png)*

*除此之外，他们还研究了用于量化公平性的指标。在论文《 [*关于语境化语言表征的内在和外在公平性评价指标*](https://www.amazon.science/publications/on-the-intrinsic-and-extrinsic-fairness-evaluation-metrics-for-contextualized-language-representations) 》中，他们表明用于衡量公平性的常用指标反映了他们数据集的偏差——*

*![](img/dd2a43a932ad0d28adaba3f2763e2d85.png)*

*为了解决这个问题，亚马逊创造了一些自己的衡量标准。*

*![](img/258673932f9f7935f80ea1dbff2d3281.png)*

*FROM “[MEASURING FAIRNESS OF TEXT CLASSIFIERS VIA PREDICTION SENSITIVITY](https://www.amazon.science/publications/measuring-fairness-of-text-classifiers-via-prediction-sensitivity)”*

*为了克服公共数据集中的性别偏见问题，亚马逊实施了以下程序-*

> *我们对基于反事实角色转换的基础知识提炼提出了两个修改——修改教师概率和增加训练集。*

*-来源，[通过反事实角色转换减轻提取语言模型中的性别偏见](https://www.amazon.science/publications/mitigating-gender-bias-in-distilled-language-models-via-counterfactual-role-reversal)*

*同样，数据扩充似乎是一个非常重要的元素。在这种情况下，它用于平衡底层数据分布。通过这样做，他们能够创造出更加公平的模型。*

*我将用一个有趣的观察来结束这篇文章。亚马逊用来实现的很多程序都没什么特别的。没有什么噱头，没有什么真正让你挠头的。相反，这里(以及他们的论文中)提到的大多数技术只是在非常高的水平上执行的合理解决方案。是的，AI 是一个瞬息万变的领域，变化不断。然而，许多这些改进是建立在良好坚实的基础之上的。了解它们会让你与最重要的发展保持联系。*

*![](img/0c0a9791c39039f80038e5df857367f1.png)*

*如果你喜欢这篇文章，你会喜欢我的每日电子邮件简讯[技术使之变得简单](https://codinginterviewsmadesimple.substack.com/)。它涵盖了算法设计、数学、人工智能、数据科学、最近的技术事件、软件工程等主题，让你成为更好的开发人员。 [**我目前正在进行全年八折优惠，所以一定要去看看。**](https://codinginterviewsmadesimple.substack.com/subscribe?coupon=1e0532f2) 使用此折扣会降低价格-*

****每月 800 印度卢比(10 美元)→ 533 印度卢比(8 美元)****

****每年 8000 印度卢比(100 美元)→6400 印度卢比(80 美元)****

*[你可以在这里了解更多的时事通讯](https://codinginterviewsmadesimple.substack.com/about)。如果您想和我谈谈您的项目/公司/组织，请滚动下方并使用我的联系链接联系我。*

# *向我伸出手*

*使用下面的链接查看我的其他内容，了解更多关于辅导的信息，联系我了解项目，或者只是打个招呼。*

*机器学习重要更新的免费每周总结(赞助)-[https://lnkd.in/gCFTuivn](https://lnkd.in/gCFTuivn)*

*为了帮助我了解你[填写这份调查(匿名)](https://forms.gle/7MfQmKhEhyBTMDUD7)*

*查看我在 Medium 上的其他文章。https://rb.gy/zn1aiu*

*我的 YouTube:[https://rb.gy/88iwdd](https://rb.gy/88iwdd)*

*在 LinkedIn 上联系我。我们来连线:[https://rb.gy/m5ok2y](https://rb.gy/f7ltuj)*

*我的 insta gram:[https://rb.gy/gmvuy9](https://rb.gy/gmvuy9)*

*我的推特:[https://twitter.com/Machine01776819](https://twitter.com/Machine01776819)*

*如果你想在科技领域发展事业:[https://codinginterviewsmadesimple.substack.com/](https://codinginterviewsmadesimple.substack.com/)*

*获得罗宾汉的免费股票:[https://join.robinhood.com/fnud75](https://join.robinhood.com/fnud75/)*