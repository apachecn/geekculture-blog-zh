# 将以太坊链上数据流式传输到 QuestDB

> 原文：<https://medium.com/geekculture/streaming-ethereum-on-chain-data-to-questdb-ea6b51d990ab?source=collection_archive---------6----------------------->

使用 [Infura](https://infura.io/) 、[区块链 ETL](http://blockchainetl.io/) 和 [QuestDB](https://questdb.io/) 将以太坊数据流式传输到时序数据库。

![](img/85631d69b98c281fa1259a8e98c37fef.png)

Photo by [Thought Catalog](https://unsplash.com/@thoughtcatalog?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

之前，我写过使用比特币基地 API 和 Kafka Connect 实时跟踪各种加密货币的价格。虽然价格对潜在投资者来说是一个重要因素，但像区块信息(用气量、难度)、交易和智能合约等链上数据也提供了有用的…