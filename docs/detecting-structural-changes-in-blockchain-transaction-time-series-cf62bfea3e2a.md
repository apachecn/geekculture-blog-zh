# 检测区块链交易时间序列的结构变化

> 原文：<https://medium.com/geekculture/detecting-structural-changes-in-blockchain-transaction-time-series-cf62bfea3e2a?source=collection_archive---------13----------------------->

使用 R

![](img/25a0d01d53fc87e119451a9ed7cc828c.png)

Photo by [Ian Parker](https://unsplash.com/@evanescentlight?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

B 锁链交易时间序列[通常在其平均水平和波动性上表现出](https://towardsdatascience.com/a-day-in-the-life-of-a-blockchain-eb352980ee16)显著的变化。通常，这种结构性变化只是反映了区块链用户的正常行为。但是，在某些情况下，它们可能表示区块链正受到恶意代理的攻击。后一类的[最近的一个例子](https://solana.com/news/9-14-network-outage-initial-overview)是…