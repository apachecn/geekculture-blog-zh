# 用 Python 的动量交易策略战胜市场:你也可以

> 原文：<https://medium.com/geekculture/beating-the-market-with-a-momentum-trading-strategy-using-python-how-you-can-too-18195a9b23fd?source=collection_archive---------1----------------------->

上个月我写了关于用 Python 自动收集金融数据的文章，今天我将使用从 Yahoo Finance 收集的金融数据创建一个动量交易策略。动量交易策略的目标是利用上涨，并通过在抛售前退出头寸来最小化下跌风险。简而言之，重复低买高卖以获得最大回报。对于这个策略，我将使用证券的价格和成交量移动平均线来创建买卖指标。