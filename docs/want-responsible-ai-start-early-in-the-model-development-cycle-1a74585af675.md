# 想要负责任的人工智能:在模型开发周期的早期开始

> 原文：<https://medium.com/geekculture/want-responsible-ai-start-early-in-the-model-development-cycle-1a74585af675?source=collection_archive---------27----------------------->

![](img/ee78c276ec3e0706b44fff9ec2f77220.png)

Photo by [Jan Antonin Kolar](https://unsplash.com/@jankolar?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 为什么是负责任的人工智能

## 政策含义:围绕人工智能的国家战略

加拿大是 2017 年第一个发布国家人工智能战略的国家，此后大多数国家都发布了自己的人工智能战略。您可以在以下位置阅读这些策略:

印度政府第一部分:[负责任人工智能的原则](https://www.niti.gov.in/sites/default/files/2021-02/Responsible-AI-22022021.pdf)

印度政府第二部分:[实施负责任人工智能的原则](https://www.niti.gov.in/sites/default/files/2021-08/Part2-Responsible-AI-12082021.pdf)

美国:[美国人工智能报告](https://www.nitrd.gov/nitrdgroups/images/c/c1/American-AI-Initiative-One-Year-Annual-Report.pdf)

美国 FDA: [软件作为医疗设备(SaMD)行动计划](https://www.fda.gov/media/145022/download)

欧洲:[数据伦理委员会的意见](https://www.bmjv.de/SharedDocs/Downloads/DE/Themen/Fokusthemen/Gutachten_DEK_EN.pdf?__blob=publicationFile&v=2)

欧洲:[人工智能/欧洲追求卓越和信任的方法](https://ec.europa.eu/info/sites/default/files/commission-white-paper-artificial-intelligence-feb2020_en.pdf)

澳大利亚:[人工智能行动计划](https://www.industry.gov.au/sites/default/files/June%202021/document/australias-ai-action-plan.pdf)

英国:[国家人工智能战略](https://assets.publishing.service.gov.uk/government/uploads/system/uploads/attachment_data/file/1020402/National_AI_Strategy_-_PDF_version.pdf)

英国:[人工智能和公共标准](https://assets.publishing.service.gov.uk/government/uploads/system/uploads/attachment_data/file/868284/Web_Version_AI_and_Public_Standards.PDF)

现在，如果你仔细阅读上述报告，你会观察到一个共同的模式，他们强调拥有负责任和道德的人工智能系统。这使得将负责任的人工智能实践纳入所有组织变得至关重要。

最近在 HBR 发表的一篇文章强调了负责任的人工智能治理的必要性

[](https://hbr.org/2021/09/ai-regulation-is-coming) [## AI 监管来了

### 随着公司越来越多地将人工智能嵌入到他们的产品、流程和产品中，我们面临着挑战

hbr.org](https://hbr.org/2021/09/ai-regulation-is-coming) 

## 所涉费用:

我们已经听到很多关于基于人工智能的解决方案如何为商业用户提供动力的说法。他们可以提供高精度的未来预测，这将极大地有助于带来商业价值。现在，所有这些解决方案都是为了寻找历史数据中的隐藏模式而构建的。如果业务过于关注获得非常高精度的模型，我们可能最终会整合各种可用的功能、新的最先进的库，只是为了获得 90%以上的模型精度，而没有意识到它的下游效应。

人工智能模型开发是一个高度迭代的过程，这反过来对任何组织来说都是一件昂贵的事情。如果我们在各种迭代之后开发了一个模型，只是后来意识到它不符合隐私、责任和安全指南。这将对组织产生巨大的成本影响，包括模型开发成本、模型收益损失成本、机会成本等等。

对于组织来说，在模型开发阶段之前包含负责任的人工智能的最佳实践是非常有用的。

# 负责任的人工智能的支柱:为什么要早点开始

![](img/90cc1c7aa417db3834955ac89e3840f8.png)

Photo by [Andrea De Santis](https://unsplash.com/@santesson89?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

以下是任何人工智能系统需要负责的重要方面。

## 隐私

各种基于人工智能的系统是如此之好，因为它们提供个性化的推荐，对最终用户有直接价值。对于这些系统，我们从各种数据源获取数据，其中也可能包含私人信息。有时候信息不能直接私密，但是多个特征结合起来有时候可以泄露私密信息。

现在，我们需要在建立任何人工智能模型之前，考虑我们在人工智能模型中使用的数据和特征。在此基础上，我们需要看看有哪些潜在的特征，可以直接或间接揭示 PII 信息。在机器学习模型中有多种方式来保护隐私，即通过使用差分隐私、生成合成数据、通过数据压缩等等。

现在，当我们使用上述技术处理数据时，我们可能无法获得与使用原始数据一样高的准确性。在模型开发阶段的早期，决定数据、隐私问题以及处理数据的正确方法就变得非常重要。你可以阅读这篇[文章](/georgian-impact-blog/a-brief-introduction-to-differential-privacy-eacf8722283b)了解更多关于差分隐私的内容。对于更多热心的读者，我推荐曼宁出版社出版的《保护隐私的机器学习》一书。

## 安全性

所有 IT 应用程序都应该有适用于它们的多层防御(数据、应用程序、计算、网络、边界、身份和访问、物理安全)，那么是什么让 AI 系统更容易受到攻击。它是我们建立人工智能模型的独特方式，我们使用的开源库，有时是那些人工智能模型背后的数学。攻击者想出各种独特的方法来欺骗系统。他们可以通过向实时人工智能系统提供虚假数据来扭曲训练数据，他们可以将对立的例子引入生产系统来欺骗它，等等。有时，数据科学家可能最终使用开源 python 库，其中存在潜在的漏洞。

现在，人工智能系统可能引发的许多其他网络安全风险将取决于模型训练频率/数据模式/使用的算法/使用的库等。我们在模型开发的早期阶段做出的所有这些选择。你可以在这里阅读更多关于 python 系统[中的常见漏洞，以及为什么理解这一点对于数据科学家来说非常重要。](https://www.anaconda.com/blog/why-understanding-cves-is-critical-for-data-scientists)

## 公平

任何 ML 模型都不应该有任何预设的偏见。然而，当我们使用真实世界的数据训练模型时，数据会包含偏差。由于样本偏斜、人为偏差、样本大小等原因，数据可能会有偏差。这可能导致 ML 模型预测有偏差的结果，简单的例子，由于训练数据中的偏差，学生的负载被拒绝。

现在，由于数据是罪魁祸首，对于数据科学家来说，寻找可能在模型中引入偏差的特征变得至关重要。这应该发生在开发机器学习模型之前。有多个基于 python 的库可用于处理 ML 模型中的公平性，即 Fair-learn、Themis-ml 等，请参考此处的[了解更多细节。如果你有兴趣了解这些算法背后的数学，我推荐你阅读下面的](https://techairesearch.com/most-essential-python-fairness-libraries-every-data-scientist-should-know/)[文章](https://towardsdatascience.com/a-tutorial-on-fairness-in-machine-learning-3ff8ba1040cb)。

## 透明度

这些人工智能系统中的许多都是通过使用复杂的算法来构建的，即深度学习、梯度增强等。这些算法不是非常直观和可解释的，因此解释某些预测背后的推理变得非常困难。然而，很多时候我们需要知道为什么人工智能会做出特定的决定，因此人工智能模型的透明性非常重要。现在，这是数据科学家需要了解互操作性、可扩展性以及实现这一目标的各种方法的地方。

这又是由业务问题驱动的，因此 it 必须在模型开发的开始就考虑周全。

# 结论

人工智能模型的可靠性和责任性是负责任的人工智能系统的另外两个重要方面。我将在下一篇文章中详细介绍这些内容。你也可以参考这些[模式](https://pair.withgoogle.com/guidebook/patterns)来构建负责任的 AI 系统。