# 我的 SDBA 编码训练营:第 7 集

> 原文：<https://medium.com/geekculture/my-sdba-coding-bootcamp-episode-7-25cb5671997b?source=collection_archive---------34----------------------->

很抱歉延迟到 60 人谁已经看了这些，根据媒体。

![](img/bcd498baee73c7cbc59c80d750e098a9.png)

Photo by [Susan Holt Simpson](https://unsplash.com/@shs521?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/building-blocks?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

因此，在一些夜晚，当我的消化问题和/或头痛非常严重时，我无法做任何有成效的事情，也无法入睡。那几天晚上我一直在看一个节目。不过我不会透露，因为我要说的会破坏它。这个节目实际上是为比我年轻的人准备的，但我很高兴我是作为一个刚刚 26 岁的人来看的。

有两个角色，让我们称他们为芭芭拉和布伦达。芭芭拉喜欢遵守规则，做她被告知的事情，并通过抽认卡学习。Brenda 希望在学到一些东西后立即进行实验和创新。两者都因为他们的存在方式在过去得到了奖励和惩罚。这对我来说很熟悉，来自传统的学校教育，但是利用[SDE/非学校教育](https://en.wikipedia.org/wiki/Unschooling)来学习我的 IT 技能。芭芭拉温和地催促布伦达先学习一些基础知识。在布伦达差点被杀，芭芭拉不得不来救她，然后布伦达决定找出基本的。

但是我认为我在 Node 的问题是我太多了。实际上，我以前在 Node 中做过一些工作(tbf，我更负责配置 Strapi 而不是 Node)，但那是我太快跳到“创新和实验”了。不用说，我在那场演出中表现不太好。我学到了很多，但我希望我能早点开始这个[节点](http://codewithmosh.com)课程。

所以我承认，在我坐下来学习 Node 的时候，我不得不暂时停止写这些文章。值得庆幸的是，我可以报告一些有趣的事情，让这篇文章仍然值得你去读:

我和其他人都有一个常见的困惑，就是 Node 拥有服务器能力，Express 拥有服务器能力，然后 Nginx 成为服务器。所以一个合理的问题是:你需要所有这些吗？很明显答案是肯定的，否则提起他们会很奇怪。这与他们都*能*处理休息的事实有关。肩膀不一样。实际上我对此很熟悉，因为 python 应用程序有一个等同于 Express 的东西(例如 gunicorn)。虽然我不记得 Flask 或 Django 是否有 REST 功能——尽管我肯定你可以通过某种方式通过 python 代码强制它们……无论如何，你可以坐在那里自己编写剩下的东西，[但是结果令人作呕](https://www.section.io/engineering-education/a-raw-nodejs-rest-api-without-frameworks-such-as-express/)(对我刚才链接的文章来说根本不是一个扣篮；也许在某个地方有这样做的理由，但不是为了我！).Express 处理主要的剩余部分是我的观点。

那么 Nginx 是做什么的？主要是反向代理。老实说，我仍然不太明白为什么它被称为 reverse，但它基本上只是将请求和响应送入和送出服务器。如果我理解正确的话，它实际上更多的是为了安全目的。例如，您在 Nginx 中配置了 HTTPS 的“S”部分。顺便说一句，在谷歌上搜索 HTTP 和 REST 之间的区别没有什么不好意思的(再次强调，学习基础知识):

“虽然许多人继续交替使用术语 REST 和 HTTP，但事实是它们是不同的东西。REST **指的是特定架构风格**的一组属性，而 HTTP 是一个定义良好的协议，恰好展现了 RESTful 系统的许多特性。”

——[拜尔顿](https://www.baeldung.com/cs/rest-vs-http)克

谢谢贝尔东！我还了解到阅读 [Joi API 文档](https://joi.dev/api/?v=17.4.2)并真正理解它所谈论的 wtf 是非常令人满意的。我认为这和成为一个十足的疯女人所能得到的钱一样多。即使在一年前，我也不知道我在看什么，尽管我可能会认为我知道。

事情:

[](https://github.com/snelzing/) [## snelzing -概述

### 阻止或报告将在复制到剪贴板的内容中找到电子邮件 Python 将文本文件转换为 CSV Python…

github.com](https://github.com/snelzing/) 

[https://twitter.com/@Sc00tr_Grrl](https://twitter.com/@Sc00tr_Grrl)

[](https://www.linkedin.com/in/shelby-e-baa410b3/) [## Shelby E. -职业自由职业者-我自己| LinkedIn

### 在手机和桌面软件 QA 测试方面经验丰富。也有一些自动化测试的经验。请…

www.linkedin.com](https://www.linkedin.com/in/shelby-e-baa410b3/)