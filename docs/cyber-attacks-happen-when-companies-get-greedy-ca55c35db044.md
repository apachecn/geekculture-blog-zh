# 当公司变得贪婪时，网络攻击就会发生

> 原文：<https://medium.com/geekculture/cyber-attacks-happen-when-companies-get-greedy-ca55c35db044?source=collection_archive---------61----------------------->

![](img/271bfe56eff55b32a3a4ba4f9f9098e7.png)

Photo by [Sigmund](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

昨天乘坐优步时，一个熟悉的故事发生了:司机发现我是一名软件工程师，开始一连串与技术相关的问题。

我总是尽力帮忙。有些问题与硬件相关或特定于产品，但它们通常很简单，我无论如何都可以回答。我发现许多程序员会抓住机会自命不凡地解释硬件和软件之间的区别，但事实是你需要对硬件有一个基本的了解才能成为一名成功的软件工程师(而不仅仅是一名程序员)。

听着司机小心翼翼地提出问题，逐渐变得越来越兴奋，让我觉得自己像无国界医生组织的医生，帮助得不到充分服务的人群解决他们在几个月或几年的过程中积累的问题。我们生活在这样一个时代，我们所依赖的技术变得如此复杂，以至于大多数人都不理解它。世界上的百万富翁比懂得编码的人还多。随着高级语言甚至无代码应用程序设计的日益流行，这些时刻只会变得越来越普遍。

不要误解我的意思，简化前端开发并使技术变得更容易获得是一件伟大的事情。它带来了不可估量的社会效益。如果一家 SaaS 公司能让当地餐馆老板轻松地把菜单放到网上，我完全支持。但最常见的是关于技术能做什么和不能做什么的基本问题。人们会向我推销他们对基于区块链的应用程序的想法，而他们没有花时间去了解区块链实际上是如何工作的。更糟糕的是，有时他们会浏览 CNN 的文章，并根据他们自己的想法和他们想听到的东西，推断出对区块链技术如何工作的完全不同的理解。

我最近一直在努力耐着性子看完《血丝》，范·迪塞尔在片中扮演一个感染了微芯片的僵尸，他必须说服一个神童程序员“黑掉”他，这样一个邪恶的初创公司就无法控制他的行动。我们把程序员塑造成这些精通的、巫师般的谜，部分原因是我们被要求如此盲目地相信技术。我们希望相信设计我们汽车安全系统的人永远不会犯错误，或者可以访问我们电子邮件的谷歌员工自己永远不会被黑客攻击，或者 Mark Z 值得拥有 1220 亿美元，因为他编写了一些以前不存在的创新代码。

技术盲的一个后果是，包括立法者( [*特别是*和](https://www.youtube.com/watch?v=n2H8wx1aBiQ&ab_channel=NBCNews)立法者在内的大多数公众，通常对重大事件没有足够的洞察力来自我管理和解决伦理困境。当公司遭到黑客攻击时，纳税人应该买单吗，就像他们对银行和赌场所做的那样？谁更应该受到谴责:小偷，还是通过让一个准备不足的保安负责保护一小笔财富来削减成本的高管？

![](img/ab75bf21386eb2d5d370c874ee32eadb.png)

Image: [Shutterstock](https://www.shutterstock.com/image-photo/male-security-guard-sleeping-workplace-1008333154)

在这篇文章中，我将用通俗的语言揭露像 Colonial Pipeline 攻击这样的黑客背后的肮脏细节，并解释为什么它们比我们所认为的更容易预防。剧透:一家公司越重视利润，就越有可能被黑客攻击。

**后门**

假设你是一栋公寓楼的房东。你给每个房客一套他们公寓的钥匙，但你保留一把万能钥匙以防不正常情况；比如租客不小心把自己锁在门外，或者你不情愿的要把他们赶出去。万能钥匙可以打开整栋楼的任何一扇门。但是权力越大，责任越大——小偷们知道，如果他们控制了万能钥匙，他们就能比用任何一把单独的钥匙偷走更多的东西。

有很多方法可以更好地保护您的租户，但它们都是以增加您的不便为代价的。例如，您可以实现以下内容的任意组合:

*   你可以保留每个人钥匙的副本，而不是保留一把主钥匙。小偷仍然可以偷走所有的复制品，但是他们要花更长的时间才能打开每扇门。然而，这也意味着打开每扇门需要花费更长的时间。
*   你可以把万能钥匙锁在保险箱里。但是买一个可靠的保险箱很贵，而且每次使用后取回钥匙和更换钥匙也要花更长的时间。
*   **你可以在所有的门上安装更好的锁，**这样入侵者就无法模仿万能钥匙轻易撬开锁。但是，唉，更多的成本…

现在想象一下，就像许多科技公司一样，你要对数百万扇门负责，而不是几个租户。怎样才能阻止一个心怀不满的工程师决定他们没有得到足够的报酬，并在黑市上出售万能钥匙的副本？

后门是开发人员在软件中故意制造的漏洞，用作万能钥匙。它们通常用于产品开发、测试和客户服务。

例如，假设网站[www.example.com](http://www.example.com)要求用户输入用户名和密码来访问他们的账户。然而，开发人员已经创建了一个秘密的“管理员密码”这是一个 16 个字符的字符串，如下所示:

```
mdglor9cf2s9gxl0
```

该密码适用于*的任何*用户名。因此，它可以用来解锁网站数据库中的任何帐户。一些最严重的攻击发生在没有严格的安全协议来规范后门处理的时候。因为它们是有目的地为开发人员提供方便、不受约束的权力，所以攻击者就像鸡舍里的狐狸。

事实上，后门攻击是出了名的难以检测。公司通常会在[几个月后](https://uktechnews.co.uk/2019/10/21/on-average-it-takes-206-days-to-detect-a-data-breach-heres-how-to-do-it-in-one/)才发现他们已经被黑客入侵，如果真的被入侵的话。

**IT vs OT**

信不信由你，我们可以让远程入侵变得不可能。我们该怎么做？通过使远程访问变得不可能。

独立系统是没有连接到互联网的网络，这使得黑客无法远程渗透。如果你正在运行一个石油钻井平台，你需要能够监控压力水平，分流，并与钻井平台不同部分的工人沟通。然而，每个压力表都不需要看到凯蒂·佩里的最新推特，也不需要任何水龙头来查找香蕉面包的食谱。

这些设备*可以*远程相互连接，在许多情况下，只要系统保持离线，这将非常安全和更具成本效益。[运营技术(Operational technology，](https://www.virtualarmour.com/operational-technology-vs-information-technology-differences-similarities-how-the-intermix-with-industrial-control-systems/)或 OT)是指与物理世界联系的设备，而信息技术(IT)则包含管理数据的系统，包括硬件和软件。根据海事执行官的说法，在疫情期间，技术人员绕过安全保护在家工作，导致 2020 年海上网络攻击增加了 400%。一旦他们远程连接到独立系统，IT 和 OT 系统就不再分离，黑客就有了进入的途径。

当然，物理系统可以被人入侵，但这些入侵通常更容易被发现，更不用说更难以匿名实施了。要求物理存在极大地限制了潜在攻击者在万维网上徘徊、寻找简单得分的数量。

另一个问题是 OT 极易受到惯性的影响。尽管更便宜、更高效的服务通常随处可见，但公司不会想要更新他们已经依赖了几十年的现有基础设施。更糟糕的是，“遗留系统”一直保持离线状态，以避免与 OT 连接，因此它们没有收到安全更新。当然，[更新代码可能代价高昂。](https://www.reuters.com/article/us-minneapolis-police-jp-morgan-race-exc/exclusive-jpmorgan-drops-terms-master-slave-from-internal-tech-code-and-materials-idUSKBN2433E4)

“恶意黑客”的概念让人想起黑暗房间里的贱民形象，蓝色的屏幕从下方照亮他们的脸，他们应用复杂的算法，数百万位数字在屏幕上飞舞。事实上，这看起来更像是一群俄罗斯人的恶作剧——打电话给民主党全国委员会，发现约翰·波德斯塔的电子邮件账户的密码是…“密码。”

IBM 表示，他们的“攻击性安全服务团队”负责主动识别和报告产品漏洞，音乐专业的学生比网络安全专业的学生多。

这里的趋势是，一些最严重的数据泄露与抽象数学或下一代恶意软件无关。在现实世界中，bug 可能看起来更像:

*   私人用户数据以可预测的 URL 结构或命名约定存储，任何人都可以猜测和访问这些数据(去年的 [Zoom 数据泄露](https://www.washingtonpost.com/technology/2020/04/03/thousands-zoom-video-calls-left-exposed-open-web/)就属于这种情况，其中[成千上万个记录的会议](https://www.cnet.com/news/your-zoom-videos-could-live-on-in-the-cloud-even-after-you-delete-them/)任何人都可以访问)。
*   进入一个网站的登录页面而不输入凭证，退出到*关于*页面，然后从那里转到*主页*将授予会员唯一的访问权限(或者类似的东西)。我也犯过类似的错误——当 web 开发人员从头开始创建 HTML 页面时，跟踪页面链接可能会很乏味，因为每个页面都必须正确地链接到其他每个页面和样式表。
*   过于懒散的安全措施。参见马克·Z 的[关于黑进他学校的“脸书”的报道](https://www.thecrimson.com/article/2003/11/4/hot-or-not-website-briefly-judges/)事实上，网络安全专家甚至不会认为这是“黑客行为”，只是管理员非常不负责任的系统设计导致了意想不到的后果。

罪魁祸首不是低劣的思想，而是草率、仓促的产品发布、不连贯的开发团队、对测试漫不经心的态度、技术上无知的管理者等等。音乐专业的学生能够在网络安全领域工作，因为主动识别和利用这些缺陷所需的解决问题的类型往往重视创造力和打破常规的思维等特征。

如果这似乎难以置信，那么只要看看当前围绕监管北美和欧洲被指定为“关键基础设施”的公司的立法的讨论，就能了解典型问题是什么样子的。据[华尔街日报报道，](https://www.wsj.com/articles/european-energy-sector-prepares-for-new-cybersecurity-rules-11623144602)“美国电力部门的网络安全规则包括一些具体要求，如一些设备的最小密码长度”和“报告实际或可疑网络攻击的授权。”

**该怪谁？**

人们普遍认为，当数据泄露发生时，大部分过错都在消费者一方。我们被告知不要在多个网站上使用相同的密码，不要将密码存储在一个中心位置，也不要屈服于明显的网络钓鱼骗局，然后[1 . 43 亿人面临身份盗窃和欺诈的风险](https://www.marketwatch.com/story/equifax-ceo-hired-a-music-major-as-the-companys-chief-security-officer-2017-09-15)，因为 Equifax 没有负责任地处理我们的信息。

好吧，也许这有点做作。但我知道很多人不太担心自己的脸书账户被一个孤独的恶棍黑掉，不得不删除一些帖子——众所周知，小人物可以保护自己免受这种威胁——而不是老大哥，他不能。

此外，我们通常通过关注消费者来解决问题。这主要是因为有更多的方法可以从消费者端处理网络安全问题(例如，在 Equifax 黑客攻击后，许多公司开始提供黑暗网络扫描服务，让人们检查黑客是否公布了他们的社会安全号码。没错，Equifax 就是[其中之一](https://www.equifax.com/personal/products/credit/monitoring-and-reports/)。

当谈到商业方面的网络安全时，创新的空间明显更小，因为安全存储数据和分配计算资源的 B2B 服务已经广泛存在。许多公司开始组装内部架构，试图进行垂直整合。相反，他们中的许多人最好通过使用云计算提供商来有效地外包他们的安全需求。我通常会将 AWS S3 用于云存储，但他们肯定不是唯一的名字。微软 Azure、谷歌云和甲骨文也都有受欢迎的产品。这些服务通常使用“免费增值”模式，这意味着小企业的成本可以忽略不计(尽管成本效益分析在规模上可能很快变得不稳定)。

抄近路总是更有利可图，就像把银行所有的钱放在用 3 位数自行车锁保护的内部保险箱里更有利可图一样。安全专家、装甲卡车、货币递送时间表……多么令人头痛。我们的客户更希望他们不必填写这么多表格，我们可以给他们更低的价格！

如果我们承认罪犯已经存在并将永远存在，在某个时候，责任就会转移到没有充分保护自己的公司身上。如果公司在出现问题时希望得到联邦调查局的救助或支持，它们应该坚持常识性标准，以确保事情顺利进行。