# 2022 年值得关注的数据和分析趋势

> 原文：<https://medium.com/geekculture/data-analytics-trends-to-watch-in-2022-8ad7ef447f34?source=collection_archive---------0----------------------->

## 所有酷小孩都会迟到，对吧？

我知道我们已经明显进入 2022 年，但我认为仍然值得分享我对今年数据和分析的一些预测。

这些预测很大程度上是上个月在从威尔士到伦敦的长途汽车旅行中与我的团队中的一些成员交谈的结果。这些预测，就像激发它们的对话一样，在有趣和严肃之间摇摆。

![](img/bafd92cde7a2b69afe59e58cf89939ba.png)

The future, dude. Photo by [JOSHUA COLEMAN](https://unsplash.com/@joshstyle?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/trends?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 1.数据堆栈变得小众，数据团队精疲力尽

不，不只是你；2021 年将会有*很多*新的数据创业公司涌现，每一家似乎都在帮助你的堆栈中越来越小的部分。本·斯坦西尔在[最近的一篇文章](https://benn.substack.com/p/business-in-the-back-party-in-the-front?utm_source=url)中说得很好:

> 相反，数据堆栈的前端由工具的爆炸式增长来表示，所有工具都指向稍微不同的方向。有[传统 BI](https://powerbi.microsoft.com/en-us/)；还有[现代毕](https://looker.com/)；还有[无头毕](/gooddata-developers/the-future-of-bi-is-headless-e3949bb0bf2)；还有[开源 BI](https://www.lightdash.com/)；还有基于比特币的 BI。有用于分析的[笔记本](https://deepnote.com/)、用于 SQL 的[笔记本](https://count.co/)、用于协作的[笔记本](https://noteable.io/)、用于 app 的[笔记本](https://hex.tech/)和用于笔记本的[app](https://towardsdatascience.com/introduction-to-papermill-2c61f66bea30)。有[数据可视化工具](https://www.tableau.com/)，笔记本数据可视化[，数据可视化](/@jonmmease/announcing-vegafusion-570f62207ba7)笔记本[。有针对团队的](https://observablehq.com/) [SQL 编辑器](https://popsql.com/)，针对不想写 SQL 的人的 [SQL 编辑器](https://www.metabase.com/product/)，针对雪花客户的 [SQL 编辑器](https://docs.snowflake.com/en/user-guide/ui-snowsight-gs.html)。有[协作工作空间](https://www.hyperquery.ai/)和[工具，它们将许多事情结合在一起](https://mode.com/)。有些[电子表格我们无法摆脱](https://www.tiktok.com/@miss.excel/video/7013792677523164421)和[电子表格取代了我们无法摆脱的](https://www.fastcompany.com/90716633/how-i-recreated-wordle-in-google-sheets)电子表格；有[重建的电子表格](https://equals.app/)；还有[的电子表格，但是毕](https://www.sigmacomputing.com/product/sigma/)。更多的事情[即将到来](https://www.ycombinator.com/companies/?batch=W22&batch=S21&industry=Analytics)。[1]

这种策略并不令人惊讶——创业公司必须在实现他们的全部愿景之前专注于解决小问题。令人惊讶的是，我们似乎接受了这样一种说法，即在我们的堆栈中有更多的专用工具比更少的通用工具更好。在某些情况下可能是这样，但对我交谈过的大多数公司来说，这意味着麻烦。

## 但是精疲力竭迫在眉睫…

我们的团队已经分散开来，定期为他们的大部分任务切换工具:查询数据库，分析查询的结果，可视化分析的结果，共享那些可视化的发现，构建数据模型，记录那些模型，版本控制那些模型，等等。而且[他们已经受够了。](https://blog.count.co/the-analytical-workflow-is-broken/)

给这个工作流程添加*更多的*工具是非常可怕的。正如我们从研究[3](和个人经验)中所知，我们的注意力越分散在不同的任务和工具上，我们就变得“越不理性、越不聪明、越不专注”。我担心今年会有太多的球队吸取这个教训。

# 2.数据团队将制定“任务陈述”并放弃它们[热门话题]

我在 2021 年末开始听到这一趋势的传言，并且在 2022 年的前几个月已经开始加速。创建数据团队使命陈述的冲动是可以理解的；它创造了一种目的感，一种在团队内部重置期望的方式，更重要的是，与业务的其他部分。

话虽如此，我还是忍不住翻了翻白眼。我回想起我作为分析师参加的会议，在会上我们讨论了我们团队的远大抱负，我们如何用“最高质量的数据和技术”来“实现更好的业务决策”。但是我们在这个目标上失败的原因不是我们忘记了它，而是因为它真的很难做到。

无论如何，召开使命陈述会议，但也请召开必要的后续会议，让您的团队谈论他们真正想要改变的事情，然后您可能会开始推动这一进程。

# 3.笔记本电脑和数据目录走向企业[安全赌注]

2021 年见证了笔记本和数据目录的巨大转变，因为人们不再问“为什么我需要一个[笔记本|数据目录]”，而是开始问“我应该买哪一个？”最后，我们都认识到，要做好工作，我们需要的不仅仅是仪表板，笔记本是工具箱的一个很好的补充。此外，数据目录感觉像是数据建模运动的自然结果，有助于使干净的数据和那些光滑的新笔记本容易找到。

最大的问题是今年谁会赢得这场比赛？一个现有的企业工具会像雪花和 Azure 已经做的那样尝试构建自己的笔记本吗？有人会喜欢自己的数据编目功能吗？

或者，它会成为突破企业无处不在的小人物之一吗？

拿我来说，我总是支持失败者…

# 4.数据“丛林之书”重蹈脸书的覆辙

Python 在过去的 5 年里无人能及。它已经从“新 R”变成了每个数据工程师、数据科学家甚至数据分析师的必备语言。但是这种情况会持续很久吗？

对于数据工程师来说，正如 Mehdi Ouazza 在他最近的文章中所说的那样，争夺数据工程师最喜欢的编程语言的战争还没有结束。Rust 比 Python 评价更高，有更陡峭的学习曲线，因此他们学习它可以获得更多的可信度，最重要的是，可能实际上更适合数据工程任务。

另一方面，一直使用 python 的分析师(和🐼)进行数据转换和探索从未有过的理由*不*使用它。像 Snowflake 这样的 SQL 数据仓库速度更快，并且每天都在提升传统 SQL 的能力，这使得离开 SQL IDE，用超慢的 python 进行一些转换的成本越来越高。

# 5.协作将意味着不仅仅是“谷歌文档数据”

通常，最好的创新是借来的，或者至少是受其他东西的启发。在数据方面，我们经常在许多流行工具上看到“X 代表数据”的标语。例如，dbt 的数据工程软件工程在业界掀起了风暴，因为它巧妙地将软件工程师能为数据工程师提供的最好的东西带了进来。

当我们的类比不太正确时，这种方法的缺点就出现了。在 dbt 的例子中，他们不仅仅是重新创造了软件工程师世界的*所有*方面——他们只拿走了需要的部分，让事情变得更好。他们理解数据工程与软件工程的不同之处，就像他们看到相似之处一样。

不幸的是，不是每个人都一样聪明。最近，我们被承诺“协作”的工具所淹没，但实际上他们所做的是给你谷歌数据表，然后就到此为止。没有人希望与某人实时构建 SQL 查询，向笔记本添加注释只能捕获我们日常协作的一小部分。坦率地说，这是不够的。

2022 年是有人[5]在数据合作应该是什么样子的背后提出一些创造性和分析性的想法，并真正动摇事情的一年。

# 6.有人会在元宇宙做数据…即使那里没有业务[热门话题]

向所有元宇宙爱好者道歉，这真的不是针对你的。目前，元宇宙是一个充满创新、创意和潜力的诱人绿色空间。有人会想在这个充满可能性的世界里走来走去，希望他们能用手转动数据，或者在一个高高的条形图中走来走去，或者有其他愚蠢的想法。这是元宇宙变得可访问和足够令人兴奋的一年，我们看到新应用程序的激增在元宇宙发挥作用，看看它是否有效，数据不会被落下。

老实说，我会去尝试的。

# 7.最后一英里成为下一个数据网格[安全赌注]

2021 年，神秘的数据网让世界议论纷纷。那是什么？那要看你和谁谈过了。但是你可以打赌我们都想知道。在分析的最后一英里，我听到了类似的热情和同样不同的想法。是反向 ETL 吗？是无头 BI 吗？完全不同的东西？

我们已经写了它对我们的意义[6]，但我们相信其他人会告诉你一些不同的东西。不管怎样，你都会听到很多关于今年最后一英里的事情，所以系好安全带。

# 8.我们记得面对面的数据活动对所有那些松弛的渠道(热门话题)有多好

这一年，我们都记得与有共同兴趣的人一起吃冷披萨和温啤酒的神奇之处，记得坐在不舒服的椅子上，旁边有人喷了一点身体喷雾，了解尖端工作的神奇之处。今年，我们将摆脱 2020 年 3 月加入的那些松散的频道，退订一些简讯，并试图再次找到真实和虚拟联系的正确平衡。为此干杯🍻。

# 9.自助服务找到新的激情[安全赌注]

不要称之为复出…因为它从未离开。几年前，我听说了很多关于自助服务的事情。我们都做着同样的白日梦，希望我们的业务合作伙伴能够独立完成他们的大部分分析，这样我们作为一个数据团队就可以解放出来进行更有成效的活动。然后，我们似乎都得出了一个令人震惊的结论:自助服务不会起作用，除非我们把自己的事情处理好。因此，我们花了几年时间专注于建立可靠的数据管道和讲述更好的数据故事，但现在我们准备再次向外部世界敞开大门。但这行得通吗？

你最好相信我们会尽力的！如果最后一英里像我预期的那么长，那么自助服务自然会在今年重新焕发活力。也许这一年它变得不仅仅是一个梦…

# 10.决策科学家是今年的分析工程师[热门人选]

正如我在之前的预测中提到的，今年的一大重点将是如何更好地将我们的数据整合到业务中，特别是决策制定中。对此，一个更明显的解决方案是为某人创建一个职位，以确保数据在决策中得到有效利用。提示决策科学家。

决策科学家或运筹学研究者已经存在多年，像 Meta 这样的公司已经雇佣了决策科学家[7]。但是 2022 年是决定科学家从边缘走向主流的一年。

这些特立独行的人在数据、业务知识和心理学的交汇处运作，专业地确保数据以最佳方式使用，以影响可能的最佳业务成果。简而言之，在正确的组织和结构中，他们可以发挥很大的作用。

# 结束语

即使我的这些预测完全错误，但有一件事我可以肯定:2022 年将是令人兴奋的一年。在第一个半月，我们已经看到 Twitter-verse 因数据堆栈“捆绑”的想法而变得活跃，我们看到新的工具出现，承诺探索数据的新方法，一些大型 A 轮公告承诺创新和增长即将到来。

但是你怎么看？我做对了什么？错了？我完全错过了什么？

今年，我将在我们的博客[上比数字](https://blog.count.co)更多地报道这些话题。如果你想跟上 2022 年的流行趋势，就去看看吧。

# 参考

[1]本·斯坦西尔，“[生意在后面，聚会在前面](https://benn.substack.com/p/business-in-the-back-party-in-the-front?utm_source=url)”。2022 年 2 月 4 日。

[2]泰勒·布朗洛，“[分析工作流程被打破](https://blog.count.co/the-analytical-workflow-is-broken/)”。2021 年 2 月 18 日。

[3]乔纳森·哈里(Jonathan Hari)，被偷走的焦点:为什么你不能集中注意力——以及如何再次深入思考。2022.

[4] Medhi Ouazza，“[争夺数据工程师最喜爱的编程语言的战争还没有结束](https://betterprogramming.pub/the-battle-for-data-engineers-favorite-programming-language-is-not-over-yet-bb3cd07b14a0)”。2022 年 1 月 27 日。

[5]自私的计数插头，我们回到测试阶段，正在研究这个问题。点击了解更多[。](https://counthq.typeform.com/to/lAZz58xt?typeform-source=blog)

[6]泰勒·布朗洛(Taylor Brownlow)，[现代数据堆栈，是时候给你来个特写](https://blog.count.co/modern-data-stack-its-time-for-your-closeup/)。2021 年 11 月 8 日。

[7]Meta 的一个[决策科学家](https://www.metacareers.com/v2/jobs/2919744658353835/)的空缺职位。