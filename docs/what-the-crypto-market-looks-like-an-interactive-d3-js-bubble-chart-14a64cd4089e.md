# 密码市场是什么样的？—交互式 d3.js 气泡图

> 原文：<https://medium.com/geekculture/what-the-crypto-market-looks-like-an-interactive-d3-js-bubble-chart-14a64cd4089e?source=collection_archive---------32----------------------->

![](img/4427dc90a45685c33719539fd533c0ac.png)

Photo by [Joshua Aragon](https://unsplash.com/@goshua13?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**数据可视化**是传达数据分析结果的方式。这意味着创造故事，视觉环境，使数据更容易理解。对于我们今天面临的所有大型数据集，它变得越来越重要。

作为一名对数据可视化和每一种表示发现的工具都非常感兴趣的数据分析师，我决定深入研究一下著名的 JavaScript 库 **d3.js** 。我最喜欢它的一点是，它允许你按照自己想要的方式创造自己想要的东西。您不会受到库的限制，其他可视化工具可能会有这种情况。

在这篇文章中，我将解释我是如何做这个图表的，并展示一些关于加密市场的结果。

我创建的可视化如下(可在[观察](https://observablehq.com/d/4af37b5b43cce051)上获得) :

# 加密货币市场泡沫图

**2021 年 6 月 4 日**

npogeant/observablehq.com

我用 HTML，CSS 和 JavaScript 做了这个气泡图，用的是 d3.js 库，我在最大的交互数据可视化网站 [Observable](https://observablehq.com) 上导出了我的代码。该网站允许您使用笔记本来创建 dataviz。

我想分析加密货币市场，不是因为我是一个超级密码迷，而是因为这是一个有趣的市场，有很大的变量可以放在图表中。因此，我使用了 [CoinMarketCap](https://coinmarketcap.com) API 来获取我的数据。基本上，该数据集由 2021 年 6 月 6 日网站上的前 100 个加密组成。CoinMarketCap 根据市值对它们进行排名。你的市值越多，排名就越高。

让我们传递到代码…

## 创建气泡图

我不会解释我所做的一切，代码在 Observable 和我的 github 的笔记本上，但我会强调它的主要方面。

首先，我必须处理数据，这是我不得不修改的一个例子:

```
market_cap: market_cap.replace(/\,/g, '').replace(/\./g, '').slice(0,-2)
```

的确，圆点和逗号是个问题，所以我几乎在每一列都把它们去掉了。

一旦数据连接，因为它是一个 csv，我必须创建它的层次结构，以便每个节点都可以获得参数，从而可以构建图表。

```
const root = d3.hierarchy({children: data})
 .sum(function(d) { return d['market_cap']; })
 .sort(function(a, b) { return b['market_cap'] - a['market_cap']; })
```

在上面的例子中，我用市值变量来表示我的层次结构。

然后，我创建了 svg 和所有与节点相关的圆圈(每种货币)。我输入，附加组(g)并使用“转换”属性来放置每个圆。圆圈的大小等于每行的“r”变量(在层次结构中创建)。

```
const node = svg.selectAll(".node")
 .data(pack(root).leaves())
 .enter().append("g")
   .attr("class", "node")
   .attr("transform", function(d) { return "translate(" + d.x + "," + d.y + ")"; }) node.append("circle")
       .data(pack(root).leaves())
      .attr("id", function(d) { return d.id; })
      .attr("r", function(d) { return d.r; })
```

最后，我通过添加文本来添加标签。第一个是符号变量，第二个是用于创建圆圈的变量，我决定将市值作为图表的初始变量。

```
node.append("text")
        .attr("class","labels")
        .attr("dy", ".2em")
        .text(function(d) {
            return d.data.symbol ;
        })
        .attr("font-family", "Roboto")
        .attr("font-size", function(d){
            return d.r/5;
        })
        .attr("fill", "white")
        .style("text-anchor", "middle")

  node.append("text")
        .attr("class","SecondLabels")
        .attr("dy", "1.8em")
        .text(function(d) {
            return d.data.market_capClean;
        })
        .attr("font-family", "Roboto")
        .attr("font-size", function(d){
            return d.r/7;
        })
        .attr("fill", "white")
        .style("text-anchor", "middle");
```

我还添加了一个以 mouseover、mousemove 和 mouseleave 为功能的工具提示。

最后，我希望它是交互式的，允许在泡泡中选择变量。因此，我在 svg 的不同元素上创建了一个带有一个参数和选择/转换的更新函数。

```
svg.node().drawData = function updateData(variable) {....}
```

这个函数的细节在笔记本中，更新图表数据的变量是通过 svg 上面的 4 个单选按钮获得的。

让我们看看图表本身，有趣的是注意到了什么…

## 一些结果

加密市场是特殊的，当泡沫由市值组成时，我们直接发现了比特币的优势。大写的第二种货币是以太币，价值是 BTC 的一半。

从交易量来看，美元领先是正常的，因为美元是最稳定的货币。比特币和以太坊成交量之间的差异比资本化之间的差异要小得多。

从价格泡沫中，我们注意到了不同的东西。首先，一些硬币是基于比特币的，这解释了为什么比特币不是价格第一的货币。排名第一的是 YFI，这是一种由 iEarn 创造的代币，这是一种发现收益率波动并允许更多流动性的金融工具。

最后的结果来自流通中的硬币供应。我们可以直接发现一枚硬币大大超过了其他硬币:柴犬(SHIB)。这枚硬币几乎和 Dogecoin 一样，它是一种仅仅基于柴犬形象的代币。这是一个迷因硬币，价格非常低，热情很高，这解释了大量供应的原因。

总之，创建这个图表真的很愉快，我学到了很多东西，也看到了 d3 是如何构建的。

我希望这是有趣的，你学到了很多东西！