# 居尔登能源消耗，与比特币的真实对比

> 原文：<https://medium.com/geekculture/gulden-energy-consumption-an-honest-comparison-to-bitcoin-5c32cdac9793?source=collection_archive---------8----------------------->

加密货币的能源使用是目前每个人心中的热门话题。

这两个极端的说法都有点愚蠢，区块链极端分子声称人们应该完全忽略能源使用，而反区块链极端分子则希望利用这一点来禁止所有的加密货币，而不像居尔登那样探索提供替代解决方案的选项。

在这篇特别的文章中，我不打算站在哪一边，我只想说，真理总是在中间的某个地方，我们都应该在某种程度上关心这些事情，并形成自己的观点。

每个人都在思考这些话题，最近很多人都在问我关于 Guldens 能源使用和能源效率的问题。

在本文中，我试图尽可能公平地比较古尔登和比特币的能耗。

## summary/TL；博士；医生

有很多因素需要考虑，包括当前与未来的块奖励和网络参数，但考虑到与比特币相同的市值(如果居尔登取代比特币)…

通过一些微调，居尔登网络至少能够实现比特币 260 倍以上的效率，甚至可能更高，同时仍然保持同等或可能更高的安全性。并且有可能为能源使用设置上限，而不是永久增长。一个不仅在资金方面，而且在环境方面都能以较低成本运行的网络。在绝对最坏的情况下，如果其他方面完全不变，在相同市值的情况下，它仍然可以实现比特币的四倍效率，但相比之下安全性却大大过高。

## 两枚硬币的基本信息

比特币当然是基于权力的，在撰写本文时，它的价格约为每枚 50'779 美元，最终总供应量为 18'708'768 枚，市值为 950'202'346'596 美元，网络散列率为 175'000'000 Th/s。矿工每块获得 6.25 BTC 或 317'368 美元。平均每 10 分钟发现一个块。

比特币主要是由 ASIC 挖掘的；antminer S9i 的哈希速率为 14Th，能耗为 1320 瓦。最新和现代硬件之间的性能差异很大，相比之下，老一代矿工 antminer S7 的功耗为 1293W，每秒 4.73 秒，或者在相同瓦数的情况下，大约只有 33%的性能。

居尔登利用我们自己的共识模型 [PoW](/@MacLeod_MJ_za/pow²-explored-a-post-launch-look-at-some-of-the-security-implications-2773fc11e50c) ，它有一个像比特币一样的 PoW 组件，还有第二个[独特的见证组件](/@MacLeod_MJ_za/beginners-guide-to-witnessing-11253afd645d)，它与 PoS 有很多相似之处。 [PoW](/@MacLeod_MJ_za/pow²-explored-a-post-launch-look-at-some-of-the-security-implications-2773fc11e50c) 与许多其他“替代”共识模型的不同之处在于，如果相同的简单断言成立，它的安全性可以被证明成立，并且不容易受到通过其他方式的额外攻击。更多关于此[的技术信息请点击这里](https://github.com/Gulden/gulden-official/raw/master/technical_documentation/Gulden_PoW2.pdf)和[这里](/@MacLeod_MJ_za/beginners-guide-to-witnessing-11253afd645d)。

在撰写本文时，居尔登的价格为 0.032 美元，最终总供应量为 750，000，000 枚硬币，市值为 17，000，000 美元，网络散列率为 5，000 Mh/s。采矿者获得的奖励为每块 50 NLG 或每块 1.6 美元，但很快将降至每块 20 NLG 或每块 0.64 美元。平均每 2.5 分钟发现一个块。

居尔登仅由 CPU 开采，这是少数几个 CPU 开采的硬币之一，由于只有通过我们的见证系统才有可能实现的协同作用，它成功地经受住了时间的考验。一个合理的“高端”CPU 可以以大约 10Mh/s 的速度工作，功耗大约为 300 瓦。尽管各种 CPU 的哈希速率不同，但性能功耗比特性大致相当，只是略有波动。

得益于更高效的交易格式，居尔登可以在相同的块大小中容纳多达两倍的交易量。

## 简单/幼稚的比较

据计算，居尔登目前的能源使用量约为 150 千瓦。

如果完全是最新一代的硬件，比特币可以说是 16'500'000 千瓦，因为它不是，我们再加上额外的 30%(实际上可能更多)，留给我们的数字是 21'450'000 千瓦(尽管一些估计认为这要高得多)。

因此，居尔登的能效是比特币的 143，000 倍，如果我们考虑每块交易容量的能耗，则是比特币的 286，000 倍，赶快停止印刷，告诉所有人！

嗯，不，如果我们是一个普通的骗局硬币，这正是我们会做的，但在居尔登，我们比这更好，所以让我们深入为什么这是错误的。

很简单，居尔登目前的估值比比特币低很多；能源使用与价格成比例，所以如果居尔登明天取代比特币，这些数字会突然看起来很不一样，因为更高的价格会推动更高的能源使用。

## 稍微不那么幼稚的比较

那么我们如何比较呢？嗯，我们可以看看如果古尔登斯价格上涨会发生什么，我们可以考虑如果 1 NLG 等于 1 BTC 会发生什么，但这不太可能，因为居尔登的供应量比比特币多得多。因此，正确的比较应该是他们的市值是否相同。

因此，让我们来看看这一点的影响…
在市值为 950'202'346'596 美元的情况下，1 居尔登将价值 1267 美元，是之前的大约 39618 倍。
每个区块有 20 个居尔登(25340 美元)，我们预计网络散列率将攀升至 79237000 千瓦，能耗将达到 2971387 千瓦。大约是比特币的 14%。

虽然有些人可能会说，相反，我们应该简单地看看美元每小时，并假设纯原始能源的使用；在比特币上是 1904208 美元/小时，而在居尔登是 608160 美元/小时。大约是比特币的 32%。

第一个比较价格在其他市场外部性，硬件成本，设置等，而第二个假设只有能源。虽然我认为第一种稍微更准确一些，但事实可能介于两者之间，所以我们可以说居尔登比比特币的效率高 25%，或者说在同样的价格下，使用大约 1/4 的能源。或者 50%又 1/8，如果我们考虑交易容量的话。

仍然是一个进步，但不像以前那样令人兴奋，然而这仍然不是完整的画面，因为它没有考虑到共识模型的差异。实际上有必要消耗这么多能源吗？真的安全吗？任何硬币都可以声称其能效无限提高，只要它们不需要支持自己的安全模型。

## 比较的更好尝试

为了更好地进行比较，我们需要考虑居尔登的安全模式。更多关于这个[这里](/@MacLeod_MJ_za/pow²-explored-a-post-launch-look-at-some-of-the-security-implications-2773fc11e50c)。

比特币的安全模型相对简单，保护网络的能源(和硬件)越多，它就越安全，价格和能源使用量必须随着时间的推移不断上升，否则最终网络可能会受到 50%的攻击，如果使用廉价的闲置采矿硬件和电力支出来造成危害的成本/利润比使用它来采矿更有利可图。虽然一个矿工可以同时做这两件事，这使这个等式更加复杂。
无论如何，尽管有一些减少能源使用的空间(如果采矿奖励降低而价格不提高)，但在重大问题出现之前，人们可能最多只能减少一半或四分之一。无论如何，这是不可能的，因为比特币开发者和社区不太可能允许在原来的一半之外改变奖励。节能通常伴随着价格的上涨，抵消了节能的潜力。

Guldens 安全模型更复杂，攻击的代价由 PoW 挖掘和见证参与共同决定，并且是两者的乘积。即攻击居尔登比攻击具有相同能量使用的 PoW 硬币更昂贵/困难，并且也比攻击纯 PoS 硬币更昂贵/困难。

在我们目前 150 千瓦的微薄能量使用量下，我们已经很安全地应对了一次比许多高市值能量币花费更多的攻击，这些能量币具有更高的能量使用量。

与比特币不同，我们也不像比特币那样一成不变，我们已经并将继续做出改变，当这些改变得到社区的共识并改善事情时。
社区已经多次表明其意图，即如果价格大幅上涨，奖励应该降低，现在我们正在实施减半以帮助实现这一点，尽管我的意图是提出一种随着价格上涨降低采矿奖励的更动态的方法以供讨论。

虽然如果我们的价格上涨，我们肯定希望我们的安全性比现在增加一点，但是每块 25340 美元将带来的安全性水平远远超过任何合理要求，因此可以推测，如果居尔登价格上涨到 1267 美元，块奖励可以降低到 1 NLG，同时仍然在大约 118800 千瓦的恒定能量使用量下提供足够的安全性。

这将代表一种网络，其不仅具有 0.5%能量使用的较低能量消耗；效率提高 180 倍，但运行成本更低，安全性更高。如果考虑到事务处理能力，这一比例甚至更低，约为能耗的 0.25%，效率提高了 260 倍。有很大的安全余量。

## 技术计算

上面我给出了一些安全数字，但是如果没有计算出来，有些人可能会认为我是凭空捏造的，所以在这一部分我展示了我的计算。

居尔登目前的证人总重量约为 1279689223，为了谨慎起见，我们将假设其中的一半 639'844'611 1%的重量为 6'398'446，这代表大约 500'000 NLG 被关押一年。

下面的内容忽略了硬件上的硬件资本支出，我留给读者一个练习。我们假设硬件是“免费”的，因为两者之间的硬件差异虽然是一个有趣的话题，但并没有改变太多事情，并且给已经复杂的讨论增加了太多复杂性。

关于下面的数字是如何得出的更多阅读见[这篇文章更多细节](/@MacLeod_MJ_za/pow²-explored-a-post-launch-look-at-some-of-the-security-implications-2773fc11e50c)。

请注意，下面忽略了一个攻击矿工仍然获得块奖励(这是一个有利于居尔登的因素，因为它的利润比比特币低)，以及硬币之间的块时间差异，这意味着对古尔登的 10 形态攻击将是对比特币的 3 形态攻击，但其他方面变化很小。

**$25340 奖励每块场景:**

在居尔登，攻击 10 个 confirms 需要 10 个这样的账户，每块 25340 美元的奖励是一整年锁定的总资本支出 63.35 亿美元！；并且仍然需要 100 倍(或者可能更多)的网络哈希速率，或者大约 297'138'700 kW/h 的能量消耗(攻击比特币所需能量的 13.8 倍),这意味着每千瓦时增加 0.06 美元，与比特币每小时 645'953 美元的成本相比，这几乎是每小时 17'828'322 美元的成本。这仍然忽略了硬件上的资本支出，我留给读者一个练习，我们假设硬件在这里是“免费的”。

**$1267 奖励每块场景:**

在每块 1267 美元奖励的情况下，由于价格相同，资本支出仍然保持在 6335000000 美元！；并且仍然需要 100 倍(或可能更多)的网络哈希速率，或大约 11'885'500 W/h 的能量消耗(攻击比特币所需能量的一半),这意味着每千瓦时 0.06 美元的额外成本，几乎是每小时 713'130 美元，而比特币每小时 645'953 美元。

考虑到额外的见证资本支出以及其他安全方面[在此详细讨论](/@MacLeod_MJ_za/pow²-explored-a-post-launch-look-at-some-of-the-security-implications-2773fc11e50c)公平地说，在这个奖励水平上，居尔登在安全性方面大大超过了比特币，在这一点上，攻击的能耗仍然比比特币多一点。

![](img/e818d6c5c34304bb998e11167db91477.png)