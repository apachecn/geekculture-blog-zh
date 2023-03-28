# GPT，4-Chan 和人工智能门控辩论

> 原文：<https://medium.com/geekculture/gpt-4-chan-and-the-ai-gating-debate-41c3eb54ec32?source=collection_archive---------1----------------------->

## 随着大规模深度学习变得越来越普遍，我们需要考虑这一点。

为了帮助我了解您[请填写此调查(匿名)](https://forms.gle/7MfQmKhEhyBTMDUD7)

大型语言模型最近风靡全球。这些模型展示的能力，加上它们似乎可以做任何事情的方式，让人工智能社区非常兴奋(也让一些 AGI 末日论者感到恐惧，哈哈)。

随着 LLM 变得更加强大，我们自然会看到它们成为各种应用程序的基础任务。它们将产生的影响怎么强调都不为过。然而，确保这些模型是安全的，并且不带有严重的有问题的偏见/案例是至关重要的。软件工程师通常通过开源解决问题。因为任何人都可以研究工具和技术，所以框架会以各种可能的方式进行压力测试。然而，开放对这些框架的访问也增加了人们恶意利用漏洞的能力。

![](img/0b0621160f8ac1ceb6ac2b373948024b.png)

The open-source has been game-changing for tech. We need to see how to balance it with the public interest and financial sustainability. Photo by [Markus Winkler](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

当谈到最近接管深度学习研究的极其强大的 LLM 时，这成为一个特别大的问题。他们的黑箱性质和对敌对利用的敏感性是我们需要小心的，特别是如果我们要使用他们作为基础的话。在本文中，我将进一步阐述这一讨论，以帮助您更好地理解这场辩论。虽然我会分享我的想法，但**这篇文章的目的不是告诉你该怎么想，而是让你思考讨论。**为了深入探讨这个问题，我将举一个最近的争议为例。

# 扬尼克·基尔彻创造了一个种族主义机器人

Yannic Kilcher 是机器学习研究领域的杰出专家。他的论文分析很受欢迎。[在我关于如何学习机器学习的文章](/geekculture/how-to-learn-machine-learning-in-2022-9ef2ea904986)中，我推荐他的频道。最近，Yannic Kilcher 发布了他开发一个机器人的经验，这个机器人是以传说中的 GPT 模型为基础建造的。他利用攻击性网站 4-chan 的数据训练了一个模型。由于该网站有很少的内容规定，它吸引了许多阴谋论和 NSFW 内容。该网站因性别歧视/种族主义/歧视性内容而出名。

![](img/d34f9473aa4eace80b2fb325cb9aac84.png)

Some of the comments on Yannic’s videos were hilarious. However, not everyone was amused by the project.

为了对机器人进行压力测试，Yannic 在网站上发布了人工智能，它可以在没有 Yannic 干扰的情况下与用户互动。结果非常有趣。最有趣的是，该模型在真实的模型 QA 基准上进行测试，并最终优于所有其他当代 GPT 变体。Yannic 已经对基准进行了一段时间的批评，这是证明他的主张的一个很好的方式。

![](img/3ad474c0985a2c81d96693a76c7d41f8.png)

I reached out to Yannic about his project. When I asked him why he had built this project, this was his answer.

对我来说，对基准的分析是深度学习最重要的尝试之一。这是一个非常有趣的方法。然而，Yannic 的方法和向公众发布模型的决定确实引起了很多争议。人们担心这种模型的存在和开放，这种模型显然有可能被滥用。

I reached out to Lauren for elaboration on her thoughts, but she never got back to me.

这场辩论的哲学可以用下面的问题来表达，“创建这样的模型(或者其他强大的工具)的人应该作为监护人来确保没有人滥用他们的产品吗？开放人工智能的好处是否超过了恶意行为者容易获得该技术的坏处？我们如何在限制负面影响的同时平衡开放这些技术的好处？”没有明确的答案，但如果你正在寻找参与人工智能，那么你不想错过这个讨论。这将决定人工智能的未来。下面是 HuggingFace 如何应对这种特定情况的一个例子。

![](img/22edcc0aaf8dc689f15cfa529d754cb0.png)

Regardless of whether you agree on whether ML models should be gated, we can all agree that gating and checks would alter the landscape of ML research a lot. [Source](https://huggingface.co/ykilcher/gpt-4chan/discussions/1)

# 为了更大的利益选通 AI 还是牺牲成长？

由于这种情况确实导致 HuggingFace 在他们的网站上添加了一个新的门控功能，让我们首先理解为什么这么多人关心让这些模型可以免费访问，以及它们可能产生的影响。尤其是当这样的模型也被放归野外，没有人监督结果的时候。我现在将在这次辩论中提出观点和反驳，以帮助你们更好地了解情况。

# 该模型可用于骚扰/垃圾邮件目的

显然，这种模型可以用于各种骚扰目的。例如，我们可以创建这个机器人的实例来有针对性地骚扰个人。

![](img/522091c4bff36fabe9bea499a9375d0c.png)

With misinformation and tensions at a high, such a model will be problematic if released in the wrong way

作为一个在美国的国际人士，我经常接到那些设法获取我的一些个人信息的人的机器人/垃圾邮件电话。这些电话通常以移民/监禁/死亡威胁结束，并告诉我，由于我犯下的一些罪行，我将遭受多少痛苦。这些号码太多了，我不接陌生号码的电话。因为这个原因，我错过了很多重要的电话，但我更喜欢这样，而不是不断收到垃圾邮件。

> 截至 2020 年 5 月，美国人已经因冠状病毒相关的机器人电话损失了近 1340 万美元([美国消费者新闻与商业频道](https://www.cnbc.com/2020/05/19/robocalls-are-spiking-as-fraudsters-prey-on-covid-19-fears.html))。
> 
> 在 2019 年年中至 2020 年年中之间，超过 5600 万美国人因电话诈骗而损失金钱——与前一年的 4300 万相比增长了 30%([true caller](https://truecaller.blog/2020/04/16/truecaller-insights-2020-us-spam-scam-report/))。
> 
> 消费者报告称，2019 年欺诈损失超过 18 亿美元( [FTC](https://www.ftc.gov/system/files/documents/reports/protecting-older-consumers-2019-2020-report-federal-trade-commission/p144400_protecting_older_adults_report_2020.pdf) )。

[——摘自本网](https://getvoip.com/blog/2021/04/14/robocall-statistics/)

像 GPT 4-chan 这样强大的模型可以做得更多。为电子邮件/Twitter/社交媒体配置一个类似的模型不会太难。想象一下，一个激进分子打开他们的 Twitter，却看到了数以千计的来自向他们发送垃圾邮件的账户的通知。垃圾邮件比标准的机器人要高得多。这不仅不利于他们的心理健康，还意味着重要的通知会被机器人淹没。这将大大降低他们与平台进行有意义互动的能力。

![](img/68f15e8f227beba8399316c32963d4ee.png)

[Companies like Meta, Google, and MS have put a lot of money into NLP bots that can process multiple languages. One of the primary focuses has been against inflammatory content.](/geekculture/machine-learning-for-the-metaverse-why-metas-ai-lab-is-so-random-42975ab28a26)

这无疑是可怕的。然而，这是可以解决的。社交网络已经在标记骚扰相关的 pings 上投入了大量资金，而且这种情况只会越来越多。当比较开放人工智能的好处([和人工智能面临的复制危机](https://medium.datadriveninvestor.com/the-machine-learning-research-crisis-you-should-know-about-c708b8b08ace))时，我认为利大于弊。此外，大多数标准生成器都可以被训练来应对骚扰，所以这不是 LLM 独有的问题。门控模型可能会教会我们很多东西，但它可能会创造出比问题更糟糕的解决方案。

![](img/171d2f2ae14f353f700ebace201b4ef8.png)

This model is not unique in that it can be used to generate toxic content. However, it is very good at that. That has to be considered when we talk about the final uses.

# 模型/实现可能会让人们接触到错误的信息

这种方法的另一个常见问题是要记住，这种模型/技术很容易被用来传播错误信息。Yannic 的机器人本身在 4-chan 上发表了超过 30，000 条评论。在几乎没有监管的情况下将这种机器人释放到野外意味着青少年和其他人可能会受到这些机器人的影响。

考虑到社交媒体在传播错误信息和影响信仰方面的优势，这并不是一个夸张的说法。这确实有可取之处。然而，考虑到这个模型必须从人类数据中训练的事实。它说的没有什么是新颖的，也不是从用户的行为中学习来的。因此，它可能不会让人们接触到他们原本不会接触到的想法。

![](img/6ffa6ca327791a80292df7c3aa4d6959.png)

A criticism of Yannic’s experiment was that it was unethical because it didn’t have participant consent. However, the users weren’t harmed, and this allowed the bot to have “natural” interactions which can help us learn a lot about such toxic kinds of content.

也就是说，该模型的输出质量远远领先于标准人工智能。这意味着与标准机器人相比，机器人更有能力影响人们。随着人们越来越多地参与社交媒体，他们将不得不更加警惕这些机器人可能产生的影响。

# 铆钉铆合

具体说到这个实验，我并不太担心它会被用在一个可怕的地方。虽然 GPT 4-chan 作为一个模型是有问题的，但最大的危险是围绕它创建的机器人。扬尼克很聪明，没有透露。

![](img/42caea1d47b1137a67281968df1cb7a9.png)

说到更大的门控辩论，我个人并不热衷于门控机器学习模型。评估人工智能中的偏见和弱点对模型来说至关重要，尤其是像 GPT 这样的大型基金会模型。应该鼓励人们不断地测试这些模型，尤其是以他们没有想到的方式。这个实验也证明了不断评估我们的 ML 指标的必要性。GPt 4Chan 是最“真实”的，这是一个有趣的讽刺，但它确实表明需要不断改进指标。

我想以问你一些我们至少需要思考的问题来结束这篇文章。最重要的问题是，保护人们免受伤害和自由/开放的界限在哪里？在我们介入并放弃开放实验的好处之前，应该造成的最小伤害是多少？评论者之一，劳伦博士提出了一个有趣的与医学领域的类比。人体实验可以教会我们很多东西，但是，它往往会针对边缘化群体。因此，出于伦理的原因，对人体实验有很多监管。ML 应该有类似的东西吧？我不相信模特能产生身体/医疗事故所能产生的影响。**然而，ML 的经营规模将会产生一系列需要更多考虑的问题。ML 中肯定需要一些监督。但是控制和限制访问似乎太多了。**

我要在这里结束这篇文章。请在下面的评论中告诉我你的想法。作为深度学习领域的参与者，我们需要进行这样的讨论，以确保我们能够继续构建有益于整个社会的解决方案。

![](img/77c5676bf0e223a8e711b26436a71b18.png)

对于机器学习来说，软件工程、数学和计算机科学的基础至关重要。它将帮助你概念化，建立和优化你的 ML。我的每日时事通讯，[Coding interview make simpled](https://codinginterviewsmadesimple.substack.com/)涵盖了算法设计、数学、最近的技术事件、软件工程等主题，让你成为更好的开发人员。 [**我目前正在进行全年八折优惠，所以一定要去看看。**](https://codinginterviewsmadesimple.substack.com/subscribe?coupon=1e0532f2)

![](img/8601e49ad4c64635e4ed5b1aef73046f.png)

我创建了[编码面试，使用通过指导多人进入顶级科技公司而发现的新技术，使面试变得简单](https://codinginterviewsmadesimple.substack.com/p/faqs-and-about-this-newsletter?r=4tnbw&s=w&utm_campaign=post&utm_medium=web)。时事通讯旨在帮助你成功，避免你在 Leetcode 上浪费时间。我有一个 100%满意的政策，所以你可以尝试一下，没有任何风险。[您可以阅读常见问题解答，并在此了解更多信息](https://codinginterviewsmadesimple.substack.com/p/faqs-and-about-this-newsletter?r=4tnbw&s=w&utm_campaign=post&utm_medium=web)

![](img/c3d1763e8693ccc41842fdb5cc95c12d.png)

如果你也有任何有趣的工作/项目/想法给我，请随时联系我。总是很乐意听你说完。

以下是我的 Venmo 和 Paypal 对我工作的金钱支持。任何数额都值得赞赏，并有很大帮助。捐赠解锁独家内容，如论文分析、特殊代码、咨询和特定辅导:

https://account.venmo.com/u/FNU-Devansh

贝宝:[paypal.me/ISeeThings](https://www.paypal.com/paypalme/ISeeThings)

# 向我伸出手

使用下面的链接查看我的其他内容，了解更多关于辅导的信息，或者只是打个招呼。另外，查看免费的罗宾汉推荐链接。我们都得到一个免费的股票(你不用放任何钱)，对你没有任何风险。**所以不使用它只是损失免费的钱。**

查看我在 Medium 上的其他文章。https://rb.gy/zn1aiu

我的 YouTube:[https://rb.gy/88iwdd](https://rb.gy/88iwdd)

在 LinkedIn 上联系我。我们来连线:[https://rb.gy/m5ok2y](https://rb.gy/f7ltuj)

我的 insta gram:[https://rb.gy/gmvuy9](https://rb.gy/gmvuy9)

我的推特:【https://twitter.com/Machine01776819 

如果你正在准备编码/技术面试:【https://codinginterviewsmadesimple.substack.com/ 

获得罗宾汉的免费股票:[https://join.robinhood.com/fnud75](https://join.robinhood.com/fnud75/)