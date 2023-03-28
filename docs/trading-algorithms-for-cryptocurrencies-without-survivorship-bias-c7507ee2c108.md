# 无生存偏差的加密货币交易算法

> 原文：<https://medium.com/geekculture/trading-algorithms-for-cryptocurrencies-without-survivorship-bias-c7507ee2c108?source=collection_archive---------29----------------------->

在 Quantiacs，我们正在举办一场以加密货币为重点的量化交易算法竞赛，我们提供数据和工具来避免生存偏差。在这篇文章中，我们描述了比赛的主要特点。

**存活偏差**是样本选择偏差的一个例子，当数据集仅包含现有(存活)观察值，但不考虑那些不再存在的观察值时，就会发生这种偏差。

这个问题在金融和算法交易领域众所周知，通常会导致**过于乐观的预期**。设想开发一个股票交易系统。考虑到大量的资产，习惯上定义一个子集用于研究和开发。一个合理的选择是从美国最具流动性的股票开始，并使用一个**指数作为选择标准**:例如，一个定量分析师可以决定将她的/他的研究集中在 [S & P500 指数](https://en.wikipedia.org/wiki/S%26P_500)的所有成员上。

让我们想象一下，定量分析师使用指数的当前成员，并使用他们的历史数据进行研究:可能一个简单的只做多策略会表现得非常好，因为定量分析师已经隐含地选择了当前的赢家进行分析。

正确的历史研究需要包括所有在过去的某个时间点是 S&P500 指数的一部分，但后来被摘牌或消失的股票。

同样的问题也影响着**加密货币**:通常我们会关注目前表现最好的，但会忽略那些在取得初步成功后消失的。

![](img/42eceda380c5a63fcd47d24b327323e9.png)

Photo by [Bermix Studio](https://unsplash.com/@bermixstudio?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/crypto?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## **quanti ACS Q16 大赛**

当前的 Quantiacs Q16 竞赛聚焦于一组**无生存偏差的加密货币**构建如下:

*   我们提供 2014 年 1 月 1 日的**加密货币数据；**
*   我们通过使用来自 [coinmarketcap](http://www.coinmarketcap.com) 的数据，根据**市值**考虑排名前 10 位的加密货币，在给定时间点定义一个**“流动”世界**；
*   我们纳入了自 2014 年 1 月 1 日以来**至少有一次**位列市值前 10 名的所有加密货币的数据；
*   我们提供了一个**过滤函数**，它为每个时间点选择属于“液体”宇宙的资产。

对于入门，我们建议在 [Quantiacs](http://www.quantiacs.com) 创建一个帐户，并从 **Q16 快速入门 Python 模板**开始，该模板在我们的 [github](https://github.com/quantiacs/strategy-first-crypto-daily-long/blob/master/strategy.ipynb) 页面上公开提供。

## Python 代码

简单模板的完整 Python 代码如下:

```
**import** **xarray** **as** **xr**

**import** **qnt.ta** **as** **qnta**
**import** **qnt.backtester** **as** **qnbt**
**import** **qnt.data** **as** **qndata**

**def** load_data(period):
    **return** qndata.cryptodaily_load_data(tail=period)

**def** strategy(data):
    close = data.sel(field="close")
    is_liquid = data.sel(field="is_liquid")
    sma_slow = qnta.sma(close, 200).isel(time=-1)
    sma_fast = qnta.sma(close, 20).isel(time=-1)
    weights  = xr.where(sma_slow < sma_fast, 1, 0)
    weights  = weights * is_liquid
    weights  = weights / 10.0
    **return** weights

weights = qnbt.backtest(
    competition_type = "crypto_daily_long",
    load_data = load_data,
    lookback_period = 365,
    start_date = "2014-01-01",
    strategy = strategy
)
```

该代码由 4 部分组成:

*   首先，我们加载需要的 Python 库， [xarray](http://xarray.pydata.org/en/stable/) 和 [**Quantiacs 库**](https://github.com/quantiacs/toolbox)；
*   接下来，我们定义一个每天加载加密货币数据的函数。函数**加载所有加密货币的历史数据**，这些加密货币至少有一个[足够流动](https://quantiacs.com/contest)；
*   定义策略的函数是一个平凡的仅长交叉系统。我们使用“收盘”字段来定义用于交叉的平均值，并使用逻辑字段**“is _ liquid”**来标记在给定时间点根据市值排名前 10 位的加密货币。计算权重后，对于不在前 10 位的加密货币，过滤器将位置设置为 0。
*   最后，我们调用内置的**回溯测试器**来计算分配权重。

这个简单的例子可以作为更复杂想法的起点。**流动性过滤器**可以很容易地应用到我们为过去的竞赛提供的其他例子中:

*   一个改进的 [**趋势跟踪系统**](https://github.com/quantiacs/strategy-futures-trending-custom-args/blob/master/strategy.ipynb) ，具有针对不同资产的自定义参数；
*   使用脊分类器的基于机器学习的系统；
*   组合分类器并执行 [**再训练**](https://github.com/quantiacs/strategy-ml-voting-crypto/blob/master/strategy.ipynb) 的机器学习系统。

截止到 2021 年 10 月**日**截止，在实时评估阶段结束后， **2M 美元**将分配给最佳系统。

你有问题吗？在我们的 [**论坛**](http://www.quantiacs.com/community/) 上写我们吧！