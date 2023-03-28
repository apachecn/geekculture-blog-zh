# 不要买便宜的德隆基浓缩咖啡机

> 原文：<https://medium.com/geekculture/dont-buy-a-cheap-delonghi-espresso-machine-8e729e466ca1?source=collection_archive---------3----------------------->

## 咖啡数据科学

## 数据驱动的审查

我没打算告诉你这台机器是台糟糕的机器。我有机会尝试它，我真的想用我所有的浓缩咖啡知识，看看我如何能得到最好的浓缩咖啡。我想很多人可能会购买这台机器作为一个介绍性的咖啡机，也许有一些事情你可以改进，以充分利用它。

然而，我无法进步，我现在已经确信这台机器不会做出好的浓缩咖啡。我确信它的设计不允许改进，而且它浪费咖啡。

![](img/418395555b3f3e76d83bf39f0ee0628a.png)

All images by author

图中的德隆基机器，价格约为 250 美元。它使用一个加压的移动式过滤器，假设你应该能够从中获得像样的浓缩咖啡，至少对于浓缩咖啡饮料来说是这样。

首先，我移除了移动式过滤器的加压部分。这可以弥补拍摄中的不足。为了能够更好地理解这一点，我需要原始的性能，但结果是，无论有没有加压部分，它仍然表现得很糟糕。也许加压移动式过滤器只是为了制造假的克莉玛。

![](img/121c66cdabd2881233a14833ca480e1e.png)

当我在做的时候，我想象了淋浴屏幕和过滤篮，以便更好地观察它们的表现。

![](img/cc39d59403edaf0d163b09400302fe8e.png)![](img/ae39252616b88483e4d3a5f8a1913232.png)

Left: Shower screen, Right: filter screen.

使用图像处理，我测量了孔的大小，我发现淋浴屏幕和过滤篮一样有几簇较大的孔。这意味着它的设计不能很好地执行。

![](img/92342ad2cb9d6a9162f27a4fd9fb81b8.png)![](img/ecb0d6919fb1d154be3843354e57f5af.png)

Filter hole analysis, colored by hole size (Yellow is the Largest, Dark Blue is the Smallest). Left: Shower screen, Right: filter screen.

我清洗了这台机器，因为它是用过的，淋浴屏幕上的图案与我想象的相符。

![](img/789d16407c6bad04cb44f277e9f02ddf.png)

# 绩效指标

我使用了一些标准来量化咖啡萃取物。我通常会加入味道指标，但根据照片的味道，它们都很糟糕，很难区分。

[](https://towardsdatascience.com/coffee-solubility-in-espresso-an-initial-study-88f78a432e2c)**用折射仪测量总溶解固体量(TDS)，这个数字结合弹丸的输出重量和咖啡的输入重量用来确定提取到杯中的咖啡的百分比，称为**提取率(EY)** 。**

****强度半径(IR)** 定义为 TDS 对 EY 的控制图上原点的半径，所以 IR = sqrt( TDS + EY)。这一指标有助于标准化产量或酿造比的击球性能。**

# **设备/技术**

**咖啡研磨机:[小生零](https://towardsdatascience.com/rok-beats-niche-zero-part-1-7957ec49840d)**

**咖啡: [La Prima 酒吧](https://laprima.com/collections/coffee/products/la-prima-bar)**

**镜头准备:常规和[断奏](/overthinking-life/staccato-espresso-leveling-up-espresso-70b68144f94)**

**其他设备: [Atago TDS 计](https://towardsdatascience.com/affordable-coffee-solubility-tools-tds-for-espresso-brix-vs-atago-f8367efb5aa4)， [Acaia Pyxis 秤](https://towardsdatascience.com/data-review-acaia-scale-pyxis-for-espresso-457782bafa5d)**

# **发射**

**我拉了几个有一些变化的镜头，我甚至试着把镜头拨进去。我知道预浸很重要，所以我能够使用 how water 功能转移水流(使用蒸汽棒)10 秒钟，给镜头一些预浸的好处。这是从一开始就计划好的，我知道我的目标是拍出最好的照片。这个蒸汽魔杖戏法是我读到的你可以为 Gaggia 经典作品做的。**

**![](img/192f0ba8f122446578bc3e41bc58c0f9.png)**

**对于机器来说，照片看起来还可以，但它们与我的超级自动相机相似。**

**![](img/e3588960244146b28bed295ce4493bf0.png)**

**从冰球的顶部，淋浴屏幕上的凹痕显示了水的来源。通常，从顶部看冰球不会显示出淋浴屏幕问题的明确证据。**

**![](img/913b0cb6be06e8244bc6280996484a8f.png)****![](img/2b37cc12fecf37d52a432818eb68d072.png)****![](img/606b1b0663a45abdac28bd0a56094894.png)****![](img/5786726c37e93a4a8b8db46150788829.png)**

**从 TDS/EY 的测量来看，拍摄速度并不快。我想拉一个更长的镜头可能会有帮助，但我不相信我看到的通灵。我有一个杠杆机拍摄只是为了给一个比较。所有镜头的比例都在 1:1 左右。**

**![](img/9f0241034dfa015d0e1f017128b80c37.png)**

**镜头拍得很快，所以我拍得更好。然而，镜头仍然在相似的时间内运行。我从底部看着冰球，我只看到过滤篮外环周围的缓慢流动。这表明水从中心流过，直接向下流动。**

**![](img/9d79ec248c3e835d8eb08e6ab754d000.png)****![](img/02422792e682469be6b2d1626b3bddf0.png)****![](img/9fc0704a257e539998e8d931047c5f07.png)****![](img/b367048ee8f2097132a38e5777bdc49a.png)**

**我对镜头进行了调整，但这似乎并没有影响性能。我怀疑这是由淋浴帘和过滤篮造成的。无论如何，我决定减少损失。如果你有这样的机器，对不起你的机器是一堆垃圾。我想知道修理这两个东西是否能帮助机器，但这不是一个人对 250 美元购买的期望。**

> **不是你的问题，是机器的问题。**

**如果你想要便宜的好咖啡，不要。要做好的浓缩咖啡，你需要好的咖啡豆、好的研磨机和好的机器(依次排列)。现在有选择得到一个接近这个价格的体面的设置(像 [Flair Espresso](https://flairespresso.com/) 和一个手动研磨机)，但不要买这个德隆基，如果你喜欢它就去升级。这台机器可以轻而易举地毁掉你的浓缩咖啡体验，因为它可能看起来就像你不知道如何做好浓缩咖啡，而不是你，而是机器。**

**如果你愿意，可以在推特、 [YouTube](https://m.youtube.com/channel/UClgcmAtBMTmVVGANjtntXTw?source=post_page---------------------------) 和 [Instagram](https://www.instagram.com/espressofun/) 上关注我，我会在那里发布不同机器上的浓缩咖啡照片和浓缩咖啡相关的视频。你也可以在 [LinkedIn](https://www.linkedin.com/in/dr-robert-mckeon-aloe-01581595) 上找到我。也可以关注我在[中](https://towardsdatascience.com/@rmckeon/follow)和[订阅](https://rmckeon.medium.com/subscribe)。**

# **[我的进一步阅读](https://rmckeon.medium.com/story-collection-splash-page-e15025710347):**

**[我的书](https://www.indiegogo.com/projects/engineering-better-espresso-data-driven-coffee)**

**[浓缩咖啡系列文章](https://rmckeon.medium.com/a-collection-of-espresso-articles-de8a3abf9917?postPublishedType=repub)**

**[工作和学校故事集](https://rmckeon.medium.com/a-collection-of-work-and-school-stories-6b7ca5a58318?source=your_stories_page-------------------------------------)**

**[改进浓缩咖啡](https://rmckeon.medium.com/improving-espresso-splash-page-576c70e64d0d?source=your_stories_page-------------------------------------)**

**[断奏生活方式概述](https://rmckeon.medium.com/a-summary-of-the-staccato-lifestyle-dd1dc6d4b861?source=your_stories_page-------------------------------------)**