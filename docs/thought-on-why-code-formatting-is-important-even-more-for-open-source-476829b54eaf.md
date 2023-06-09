# 思考为什么代码格式化对开源更重要

> 原文：<https://medium.com/geekculture/thought-on-why-code-formatting-is-important-even-more-for-open-source-476829b54eaf?source=collection_archive---------17----------------------->

![](img/027ccc127054fecb17b0c28f2c9c242a.png)

Illustration photo by [Negative Space](https://www.pexels.com/@negativespace?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/pink-white-black-purple-blue-textile-web-scripts-97077/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels).

## 格式化程序不会打扰任何人；它们旨在帮助和使任何合作更顺利，并促进代码库阅读成为一本好书的故事。

我相信总是有 n+1 种方法来编写每一行代码，但并不是所有的都一样好。同样，读者很容易反对格式化程序限制了程序员的创造力，但是这是真的吗？让我们进一步讨论一下...

**为什么谈论社区和企业？**主要原因是，公司的大多数指导方针都是由第一批加入公司的技术人员制定的。由于每个开发人员都有不同的个性，特别是公司的风格也可能大相径庭。还有，通常情况下，没有太大的变化空间；你要么接受标准，要么离开...很少有中间立场。那么为什么公司这么严格呢？他们看到了让所有开发人员使用同一种语言的价值——从长远来看，一切都会变得更加顺畅和高效。

**为什么开源/社区开发如此不同？**并不总是这样；您会发现由一小组已经调整了编码风格的程序员开发的项目，不需要任何格式强制。但是令人不安的可能是第一个不认同他们的编码价值观的外部贡献者，或者他只是有不同的背景…

![](img/5428be10b5004032ac774a248cef8d16.png)

Illustration photo by [Aleksandar Pasaric](https://www.pexels.com/@apasaric?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/photo-of-skyscrapers-surrounded-with-clouds-1437493/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels).

# 与建造巴比伦塔的类比

你也可以把那种情况从某个角度看做是一个关于建造巴比伦塔和混火钳的古老故事……项目目标是建造最伟大的塔直达天空；你必须把想一起建造它的人聚集在一起——这很好！但是现在重要的方面来了:

*   定义**的共同愿景，并设置建筑**，你可以看到有一个塔的图纸，它最终应该是什么样子。
*   **建立一个强大的基础**,可以容纳你将要构建的所有伟大的东西——在软件世界中，这意味着设计测试方案和应用持续测试。*塔楼平行:如果你错过了这一步，你的建筑可能会因为地面不稳定而在稍后开始下沉；你仍然可以使用昂贵的混凝土注射来稳定地基，但是这比开始时的 propper 设置要多得多，而且不能保证结果…*
*   在施工现场建立**适当的沟通**以提高工作效率，限制各种沟通噪音和开发人员背景造成的误解。

> 让我们把注意力集中在最后一步——建立适当的沟通，尽管剩下的两步对于成功的开发同样重要。

让我们以主要开发者和负责人的鞋子为例。现在你面临两个主要的沟通决定:

*   您将只限制使用同一种语言的参与者；尽管如此，这一决定可能会显著地分担工作组的工作，因此您应该意识到这将需要更长的时间。
*   你将欢迎所有感兴趣的贡献者，因为建设将变得更快，带来另一个有趣的想法，等等。在这种情况下，你需要管理所有人的合作——而不是花时间创造价值，而不是争论在翻译中失去了什么。

# Python 代码格式的中介

用编程语言建立一些通信标准和程序是非常相似的。让我们用 Python 作为一个例子，所有语言都坚持使用它的格式选项。各种 Python 格式化指南和工具在严格程度上有所不同——将 Python 代码从 PEP8 变成黑色。

下表列出了这些限制越来越多且限制编写自由度的级别，从 Python 解释器验证语法开始，到强制执行严格规则的黑色格式化:

![](img/7197392df151f8237d96a5d4c7a244ea.png)

Table with levelling python formatting from just passing code (top) to strict rules (bottom).

另外，大多数允许自动化的工具可以在本地使用/应用(例如，使用[预提交钩子](https://pre-commit.com/hooks.html))或者作为服务器端持续集成(CI)的一部分，以显著减少开发人员的开销。

# 个人反思

您会听到诸如“我不喜欢您的编码风格”、“不要减慢我的编码速度”之类的声音。当你近距离观察它们时，它们表达了什么？他们非常自私，只追求自己的利益，不关心其他人的长远利益。或者说他们活在自己的泡沫世界里，大家都一样(他们的路是唯一的路，其他都是错的)。

**让我们强调一下，格式规则并不意味着歧视任何人。这只是建立了一个共同的基础，所有人都可以有效地工作，贡献，为共同的目标合作，并避免无意义的争论和伤害的感情。**

确信每个同意这些规则的人都必须妥协他过去所学的和所做的。把他们看成推自己路的人是短视的，最喜欢体现自己的立场。我宁愿看看那些已经和你走过同样道路的人。他们就在前面几步，帮你一把，这样你就可以避免他们的错误，更快地过桥。

实用备注—如果您决定部署这些格式规则中的任何一个(不管是哪一个严格级别)，即使有最好的意图，而没有以任何形式强制执行它们(例如，在接受贡献之前的 CI)，就好像它们不存在一样…看看您的周围，如果没有警察检查，将会遵守多少规则:]

> 免责声明，这篇文章并不是要挑起任何格式关系的战争，我分享我的个人观点和一些实践经验…另一方面，任何建设性的讨论都是非常受欢迎的。

![](img/8d4687915cc57e3265a3f078b5eb3ab3.png)

Illustration photo by [KoolShooters](https://www.pexels.com/@kool-shooters?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels) from [Pexels](https://www.pexels.com/photo/man-reading-book-6981540/?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels)

最后一个关于格式化的平行故事:当你有不同字体和行偏移(一些字在一行之上或之下)的作品时，阅读一本没有格式化文本的书是可行的，但不存在也没有效率，所以你仍然可以阅读它，但它会花费更长的时间，并且最终会有一些混乱…任何项目代码库都是开发人员的书。

***你喜欢这个故事吗？敬请关注，关注我了解更多！***

[](/geekculture/realistic-perspective-on-release-strategies-hidden-corners-5c45b8b7cd8d) [## 发布策略的现实视角——隐藏的角落

### 从开源软件包开发和发布中获得的实践经验和教训

medium.com](/geekculture/realistic-perspective-on-release-strategies-hidden-corners-5c45b8b7cd8d)