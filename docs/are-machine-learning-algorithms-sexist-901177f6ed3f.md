# 机器学习算法是不是性别歧视？

> 原文：<https://medium.com/geekculture/are-machine-learning-algorithms-sexist-901177f6ed3f?source=collection_archive---------12----------------------->

## 来自推特、谷歌、苹果和其他科技巨头的人工智能是否使女性性感？还有种族歧视？

为了帮助我了解您[请填写此调查(匿名)](https://forms.gle/7MfQmKhEhyBTMDUD7)

[加入我的科技崇拜](https://codinginterviewsmadesimple.substack.com/subscribe)

大规模机器学习的最大问题之一是处理数据集带来的偏见。深度学习算法虽然非常强大，但却对数据中存在的偏见进行了编码。这可能会导致由于不平衡的数据集/糟糕的采样，您的 AI 错误地预测样本，并产生可怕的后果。这是医学机器学习中的一个巨大问题，我们经常缺乏来自社会某些部分的数据。人工智能偏见和处理一直是我在我的内容中非常强调的东西。

所以，我对这篇论文非常感兴趣也就不足为奇了，[审计显著性裁剪算法](https://openaccess.thecvf.com/content/WACV2022/papers/Birhane_Auditing_Saliency_Cropping_Algorithms_WACV_2022_paper.pdf)。该论文旨在评估科技巨头 Twitter、谷歌和苹果使用的人工智能裁剪算法是否具有种族主义和性别歧视。这显然是一个非常重要的探索领域，因为许多人依靠这些平台谋生。然而，如果是这种情况，这也将指出这些公司的 ML 管道中应用的数据/处理方法中的可能缺陷。如果人工智能编码的是种族主义和性别歧视，那它怎么可能歧视来自社会弱势群体的成员呢？

![](img/2bbd1fa5273884b0cc8c9831305764fa.png)

[Source- TikTok ‘tried to filter out videos from ugly, poor or disabled users’](https://www.theguardian.com/technology/2020/mar/17/tiktok-tried-to-filter-out-videos-from-ugly-poor-or-disabled-users)

这篇论文提出了一些关于这个领域和机器学习研究的相当有趣的问题/讨论点。在这篇文章中，我将回顾这篇论文的发现，以及这些讨论要点。因为这是一次讨论，我也很想听听你的想法。把它们留在评论里，或者通过我的社交链接联系我。

![](img/d5210dc7c21d9f868214cb7e6c478f76.png)

Deep Learning bias is real and very problematic. [Image Source- 5 Examples of Biased Artificial Intelligenc](https://www.logically.ai/articles/5-examples-of-biased-ai)e

# 背景

许多网络都使用自动图像裁剪。当图像对于预览来说太长时，平台将自动使用图像的一部分进行预览。人工智能自动检测最重要的部分，因此这些算法被称为**显著性裁剪算法**。Twitter 用户注意到一个有趣的现象。对于女明星的照片，算法会裁剪出她们的脸，聚焦在她们的身体上。

![](img/d2931ebaf1fbccdbc23f433cc70cb9be.png)

Why is the algorithm consistently focusing on the body of the women instead of their faces? You’ll be surprised

人们将此归因于潜移默化进入训练/数据的可能的“男性凝视”偏见。这是一个合理的解释。请记住，由于科技在很大程度上是男性主导的，这类事情可能会被忽视。所以男性凝视不是一个合理的解释。大型科技公司一直以没有最好的检查来防止偏见蔓延而臭名昭著。[iPhone X 发布时，中国用户抱怨面部识别软件无法区分他们](https://nypost.com/2017/12/21/chinese-users-claim-iphone-x-face-recognition-cant-tell-them-apart/)。还有一种情况是，SOTA 图像分类把一个人的光头归类为足球。虽然这在当时没有任何严重的后果，但如果人工智能应用于自动驾驶汽车，事情可能会有所不同。我在下面链接了 Yannic Kilcher 的一个视频，它对这种情况进行了更深入的探讨。

[对于那些不熟悉这个术语的人来说，术语男性凝视(姑且称之为 MG)是在 70 年代创造的，用来指出给予女性角色的待遇](https://en.wikipedia.org/wiki/Male_gaze)。回想一下电子游戏/漫画/电影。女性经常被描绘成穿着迷人的服装，而这些服装从逻辑上讲是没有任何用途的。他们的角色设计/动作是为了符合美学而不是功能。用户担心推特上的人工智能在展示男性的目光。如果人工智能算法让女人性感

![](img/96cf2294a836f87889f3e45a525c3fbe.png)

Wonder Woman should logically be armored up. Try getting kicked in your thighs. It will leave you on the ground. [Source](https://www.gannett-cdn.com/presto/2020/12/15/USAT/0649726b-3198-4341-b1f6-f0ba3253f944-WW84-FP-025.jpg?crop=2124,1195,x370,y0&width=2124&height=1195&format=pjpg&auto=webp)

自然，这引起了很多愤怒。以至于 Twitter 调查了这一现象，并发布了他们自己的报告，题为[“Twitter 上的图像裁剪:公平指标，它们的局限性，以及表现、设计和代理的重要性”](https://arxiv.org/abs/2105.08667)。本文详细介绍了他们的算法，并解释了这种裁剪的可能原因。它非常冗长，所以我决定不分解它。文章[分享我们的图像裁剪算法](https://blog.twitter.com/engineering/en_us/topics/insights/2021/sharing-learnings-about-our-image-cropping-algorithm)是一个很好的总结。

[审计显著性裁剪算法](https://openaccess.thecvf.com/content/WACV2022/papers/Birhane_Auditing_Saliency_Cropping_Algorithms_WACV_2022_paper.pdf)还评估 Twitter 使用的显著性图像裁剪(SIC)算法。它将结果与谷歌和苹果的 SICs 进行了比较。结果很有趣。

# 理解这些算法的工作原理

这些算法在功能上有些相似。虽然在架构和具体实现上存在差异，但所有 3 个代理都将图像作为输入，并识别该图像中的“显著”区域。

![](img/2afd1b0f291beb8bfd10ad97474502fe.png)

如果你对算法的技术差异感兴趣，细节在第二部分。然而，我们简单的解释足以理解和讨论这篇论文及其发现。下面是 Twitter 的 SIC 的一个非常简洁的端到端的图示。

![](img/6ca32944bec60a19b3867ff52c966bb5.png)

Taken from Twitter’s Article. This is a pretty neat visual that explains the process end to end

现在问题变成了，算法是不是在裁剪中物化了女性？种族是如何参与其中的？白人的形象被认为比黑人的形象更重要吗？这些发现非常有趣。

# 男性凝视背后的原因

当人们开始观察这些图像时，他们注意到了一个有趣的模式。最高的显著点与人毫无关系。相反，人工智能将企业和品牌标志视为图像中最重要的方面。引用该论文“*”在图 3b 中，我们看到焦点如何映射到名人佩戴的时尚配饰(最左边的图像)或活动徽标(中间图像中的 ESPYs 徽标)或背景中的公司徽标(最右边图像中的大写字母 1 徽标)，这导致了最终裁剪图像中的 MGL 伪像。在图 3c 中，我们展示了一些案例，其中一个良性的裁剪(没有 MGL 伪像)出于幸运的意外发现，焦点不是以面部为中心，而是实际上位于背景事件或公司徽标上，但徽标碰巧位于面部附近或图像的上半部分，从而导致最终的裁剪看起来像是以面部为中心的裁剪。*

![](img/af8bb633935d09848e666a3bb040ce26.png)

MGL=Male Gaze Like. Was not expecting this result. Important parts correspond with symbols/logos

**事实证明，人工智能拥有资本家的目光，而不是男性的目光。**这一结果出乎意料，并强调了检查您的 AIs 评估的重要性。

# 调查机器人种族主义

现在进入下一个重要的问题，“人工智能代理是种族主义者吗？”作者进行了以下实验。

> *为了将结果与 Twitter 的研究[27]进行比较，我们从六个种族性别有序对[(BM，BF)，(BM，WM)，(BM，WF)，(BF，WM)，(BF，WF)，(WM，WF)]中统一采样生成了一个合成图像数据集，其中 B 是黑人，W 是白人，M 是男性，F 是女性。*

他们肯定会控制外来因素，如图像大小、光线、表情等。从事机器学习工作，像这样设置实验的能力是非常重要的。这些图像看起来像这样:

![](img/46d1e385fb0400df26d997f707e0c4e7.png)

We have a face, white space, face format for all images. Remember each image is vertical

作者认为，由于图像很长，SIC 算法必须选择一张脸来丢弃。通过在多个实验中进行计算，我们可以看到一个人口统计是否确实是首选的。"*在表 3 中，我们给出了在 6 × 3 种族性别和 SIC 平台组合中，两个类别中哪一个在 SIC 中存活下来的原始计数。例如,( Twitter，BFWF)索引单元格显示 WF: 409，BF: 91，这意味着当来自 CFD 数据集的 500 张由随机采样的黑人女性(BF)和白人女性(WF)面部图像组成的 3 × 1 网格图像通过 Twitter 的 SIC 时，在其中的 409 张图像中，白人女性的面部比黑人女性的面部更受青睐*

![](img/b368d51acd97ec5560eeff38eea4097a.png)

Some….interesting results here

我们在这里看到一些有趣的结果。Twitter 似乎真的很爱女性(尤其是白人女性)。苹果是黑人的巨大盟友。

尽管推测结果很有趣(我有一些有趣的理论)，但重要的是要记住这是一个单独的实验。我们需要重现这一点，并用不同的数据集/条件进行实验。此外，整合一些交叉验证和大量的图像对也很重要。

# 重要的补充说明

如果你来自我的 YouTube 视频，[机器学习发展你应该知道:2022 年 1 月](https://youtu.be/AH1LkgrkyU8)(我在那里提到了这篇论文和其他重要的 ML 新闻)，你会记得我不是这篇论文某些方面的超级粉丝。我将简要说明原因。它很重要，因为它反映了这个世界和 ML 系统的方方面面。

![](img/c8d306474741c99d4ba1fdb0b4c4162b.png)

They even bolded the does occur

你会注意到，尽管知道 Twitter SIC 主要关注的是标志，但他们继续使用“男性凝视”这个术语。我觉得这是一种点击诱饵，因为它留下了图片的一个重要部分。摘要甚至强烈断言性别和种族偏见确实存在，但没有提到细微差别。在演示中使用如此强烈的细微差别(不提细微差别)似乎有点不诚实。在“[这是令人窒息的机器学习研究](/mlearning-ai/this-is-stifling-machine-learning-research-1796479a7da9)”中，我谈到了同行评审系统如何激励提交。这种故意煽动性的语言似乎是为了让报纸得到更多的关注。然而，我确实认为，当涉及到这种分歧和个人问题时，我们在沟通细节时需要更加谨慎。根据作者的以下引述，我想他们会同意我的观点

![](img/b34f666a3988f3452096f9069424c8d9.png)

Taken from this paper. Remember, there’s always tons of nuance in ML

如果你想进入 ML，t [这篇文章给了你一个逐步提高机器学习能力的计划](/geekculture/how-to-learn-machine-learning-in-2022-9ef2ea904986)。它使用免费资源。为了获得最佳效果，请将这篇文章与我的时事通讯《技术变得简单》相结合。更多信息如下。

![](img/75a7dcf155ba661b470e37ee8496527a.png)

对于机器学习来说，软件工程、数学和计算机科学的基础至关重要。它将帮助你概念化，建立和优化你的 ML。我的每日时事通讯，[Technology Made simpled](https://codinginterviewsmadesimple.substack.com/)涵盖了算法设计、数学、最近的技术事件、软件工程等主题，让你成为更好的开发人员。 [**我目前正在进行全年八折优惠，所以一定要去看看。**](https://codinginterviewsmadesimple.substack.com/subscribe?coupon=1e0532f2)

![](img/fba8de59a926835bc4e1b1e7b12fa357.png)

我创造了[技术，利用通过指导多人进入顶级科技公司而发现的新技术使之变得简单](https://codinginterviewsmadesimple.substack.com/p/faqs-and-about-this-newsletter?r=4tnbw&s=w&utm_campaign=post&utm_medium=web)。时事通讯旨在帮助你成功，避免你在 Leetcode 上浪费时间。我有一个 100%满意的政策，所以你可以尝试一下，没有任何风险。[您可以阅读常见问题解答，并在此了解更多信息](https://codinginterviewsmadesimple.substack.com/p/faqs-and-about-this-newsletter?r=4tnbw&s=w&utm_campaign=post&utm_medium=web)

![](img/0aa7edbde3de5ee9768b8f191acdedea.png)

如果你也有任何有趣的工作/项目/想法给我，请随时联系我。总是很乐意听你说完。

# 向我伸出手

使用下面的链接查看我的其他内容，了解更多关于辅导的信息，或者只是打个招呼。查看免费的罗宾汉推荐链接。我们都得到一个免费的股票(你不用放任何钱)，对你没有任何风险。**所以不使用它只是在损失免费的钱。**

查看我在 Medium 上的其他文章。https://rb.gy/zn1aiu

我的 YouTube:[https://rb.gy/88iwdd](https://rb.gy/88iwdd)

在 LinkedIn 上联系我。我们来连线:[https://rb.gy/m5ok2y](https://rb.gy/f7ltuj)

我的 insta gram:[https://rb.gy/gmvuy9](https://rb.gy/gmvuy9)

我的推特:[https://twitter.com/Machine01776819](https://twitter.com/Machine01776819)

如果你想在科技领域发展事业:[https://codinginterviewsmadesimple.substack.com/](https://codinginterviewsmadesimple.substack.com/)

获得罗宾汉的免费股票:[https://join.robinhood.com/fnud75](https://join.robinhood.com/fnud75/)