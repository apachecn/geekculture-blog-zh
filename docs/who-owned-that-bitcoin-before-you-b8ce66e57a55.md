# 在你之前谁拥有比特币？

> 原文：<https://medium.com/geekculture/who-owned-that-bitcoin-before-you-b8ce66e57a55?source=collection_archive---------14----------------------->

## 用 Python 遍历比特币的公共账本，适合初学者

![](img/8398b2ba8d14ea8837928f0efe240cb7.png)

Photo by [Freddie Collins](https://unsplash.com/@visuals_by_fred?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

几周前，我和姐姐、姐夫在约书亚树露营，我们聊了三件事:

1.  加密货币
2.  学习如何编码
3.  其他东西

虽然我姐姐非常喜欢上面列表中的第三个，但这篇博客文章是关于 1 和 2，加密货币和学习如何编码。

# 学习如何编码很难

我花了 8 年多的时间尝试学习如何编码。我的第一次尝试是《傻瓜编程》这本书，这本书是我五年级时从图书馆借的。我很快感到不知所措和羞愧，发现一本为“傻瓜”写的书实际上对我来说相当具有挑战性。

除了一次之外，之后的每一次尝试都是相似的:我想“进入编码领域”，但是不知道一旦我获得了初级水平的能力，我会做什么。结果，我总是感到沮丧并放弃，无论如何也看不到我正在学习的晦涩、混乱的东西的意义。

然而，一旦我有了一个好的动力，一切都变了。有一个大学班级承诺有一个很酷的机器人实验室，如果我能展示一些基本的编程技能，我就可以参加。另外，我所有的朋友都在服用。有一门先修课，但看起来很无聊。我又一次尝试自学如何编码…现在这是我的工作，我喜欢它。(而且，这堂课真的很有趣。)

# 本文==自主学习动机

这篇文章是为像我姐夫这样的人写的——你想学习如何编码，但你需要一个有趣的项目来帮助激励你。同样，对你来说，区块链也算“有趣”

# 这篇文章！= Python 教程

有很多好的 Python 教程，解释了如何使用该语言提供的基本数据结构和工具。这篇文章没有这样做。相反，我希望提供一个有趣的项目，人们可以用它来激励自己学习一些否则会感到枯燥乏味的东西——例如，如何访问 Python 字典上的字段。

请随意在这篇文章和更标准的 Python 教程之间来回切换，比如 Python 网站上的[教程](https://www.python.org/about/gettingstarted/)或[代码学院教程](https://www.codecademy.com/learn/learn-python-3)。您可以阅读其中的一些内容，找到您不知道如何做的一些内容，带着新的兴趣阅读更多的 Python 教程，找到宝藏，背诵咒语，感受流经您身体的魔力…并完成项目！不管怎样，这是我的梦想。

# 故事

写一个“故事”比写一个“问题”或者更糟糕的“罚单”更有趣。(这在[敏捷开发](https://www.atlassian.com/agile/project-management/user-stories)中是一件大事，我现在不会深入讨论。)这个项目的故事是:

你刚刚在 craigslist 上卖了东西给一个坚持用比特币支付的人。你想当然，为什么不呢，所以你创造了一个钱包，看着某个应用程序说他付钱给你，然后开着车走了，感觉很时髦。然而，后来怀疑悄然而至。为什么文莫不够好？买家有什么要隐瞒的吗？你谷歌一下，发现了丝绸之路，你的罪恶感倍增。如果你账户里的一小部分比特币是脏钱呢？

幸运的是，比特币是一个“分布式公共账本”，这意味着你的硬币的历史是公共信息。你的硬币来自的钱包有它曾经收到的所有信用编码在区块链中，所有人都可以看到。你所要做的就是找到一个你担心的地址——例如，这是[ilk Road wallet](https://www.coindesk.com/nearly-1b-in-bitcoin-moves-from-wallet-linked-to-silk-road)——以及你的收入支付的交易散列，你就可以验证它们之间是否存在交易链。

当然，对此有一大堆的警告，但不管怎样。这是一个有趣的项目，你可以用基本的 Python 来做。

# 设计解决方案

为现实世界的问题设计技术解决方案最困难的部分之一是知道从哪里开始。我可以帮忙。

你想遍历比特币的公共账本，以证明或反驳比特币的一些交易源自一个粗略的地址。

为此，您需要:

1.  一个 [API](https://en.wikipedia.org/wiki/API) ，您可以向它提供一个交易散列，并从它那里请求两件事作为回报:发送者地址，以及表明发送者从哪里得到钱的“以前的交易”。
2.  解析 API 响应、存储信息并再次查询 API 以任意步数遍历区块链的能力。

这听起来可能很多。如果你只是在学习编程，你可能以前没有使用过 web API。暂时不要放弃——我会尝试进一步分解解决过程，并分享我的代码，如果遇到困难，您可以使用这些代码。

在阅读我的代码之前，尝试自己解决每个部分。有时候这感觉像是一个悖论，但是学习如何编码需要你积极地写代码——仅仅阅读别人的代码不会让你走得很远。

# 寻找合适的 API

我们想要一个 API，它不仅能给我们“发件人”地址，还能给我们“以前的交易散列”，这是发送者从哪里获得资金的唯一标识符。例如，如果 Alice 支付 Bob 1BTC 减去费用，我们预计公共分类账中会出现如下内容:

```
{
 'hash': 'this_transaction_hash',
 'total': 9950000,
 'fees': 50000,
 'inputs': [{
   'prev_hash': 'prev_transaction_hash',
   'output_value': 10000000,
   'addresses': ['Alice_wallet_address'],
 }],
 'outputs': [{
   'value': 9950000,
   'addresses': ['Bob_wallet_address'],
  }]
}
```

(注意，这个例子是简化的。在一个事务对象中实际上有更多的字段，以及更难看的地址和事务散列。如果你对它们感兴趣，或者想了解比特币的一般工作原理，我推荐[掌握比特币](https://github.com/bitcoinbook/bitcoinbook)。)

我会帮你省去找一个提供合适 API 的网站的麻烦——[block cypher](https://www.blockcypher.com/dev/bitcoin/#transaction-hash-endpoint)给了我们所需要的。(为了找到它，我刚刚谷歌了比特币 API，并阅读了结果中前几名的文档。)

# 步骤零:钻孔安装

要编写自己的解决方案，您需要[安装 Python](https://www.python.org/downloads/) 。

您还应该在所有 Python 项目中使用 [virtualenv](https://packaging.python.org/guides/installing-using-pip-and-virtual-environments/) ，尽管对这个项目来说并不是绝对必要的。

或者，如果您有兴趣尝试一个有用的数据科学工具，让您一次运行几行 Python，您可以使用 [jupyter](https://jupyter.org/install) 。Jupyter 支持虚拟环境——您可以通过从命令行运行`ipython kernel install --user --name=your_env_name`将 virtualenv 添加到 jupyter 配置中。

在你喜欢的 IDE 中打开一个 python 文件(Python 自带 IDLE，所以你现在可以使用它，但是我更喜欢使用 [PyCharm](https://www.jetbrains.com/pycharm/) )，或者如果你正在调试 jupyter，从命令行输入`jupyter notebook`(更详细的 jupyter 指令[在这里](https://jupyter.readthedocs.io/en/latest/running.html))。

# 第一步:查询一个事务的 API

耶，我们可以写一些代码了！

记住，最终目标是回到过去，弄清楚你的比特币从何而来。第一步将是查询 Blockcypher 的 API 以获得关于单个事务的信息。

请随意使用您自己的事务哈希来尝试！但是如果你以前没有实际使用过比特币，或者你的钱包提供商很难找到交易哈希，你可以使用这个交易哈希:`0627052b6f28912f2703066a912ea577f2ce4da4caa5a5fbd8a57286c345c2f2`

要查询的网址是`[https://api.blockcypher.com/v1/btc/main/txs/](https://api.blockcypher.com/v1/btc/main/txs/)YOUR_TRANSACTION_HASH`。(用您的事务哈希替换您的事务哈希)。

在浏览下面的…之前，花点时间尝试用 Python 查询 API。你可能需要谷歌如何做到这一点，这没关系！谷歌搜索是编码过程的一部分。

## 第一步解决方案

下面是我为单个事务查询 Blockcypher 的 API 的代码:

```
import requestsEXAMPLE_TRANSACTION_HASH = '0627052b6f28912f2703066a912ea577f2ce4da4caa5a5fbd8a57286c345c2f2'
response = requests.get('[https://api.blockcypher.com/v1/btc/main/txs/'](https://api.blockcypher.com/v1/btc/main/txs/') + EXAMPLE_TRANSACTION_HASH)
print(response.json())
```

看，还不错！我导入了`requests`，一个用于发送 HTTP 请求的 Python 模块。然后我向 Blockcypher 的 API 发送了一个 GET 请求，并提供了我感兴趣的事务的事务哈希。我打印了响应的“json ”,它包含了从 Blockcypher 返回的成功响应的信息。

注意，如果在运行时出现类似于`ModuleNotFoundError: No module named ‘requests’`的错误，您需要通过从命令行键入`pip install requests`来安装`requests`。

结果有点难看，但是如果你把它粘贴到 JSON 格式器[中，就像这个](https://jsonformatter.curiousconcept.com/#)一样，可读性更好。对于示例事务哈希，它看起来像这样:

```
{
   "block_hash":"0000000000000001b6b9a13b095e96db41c4a928b97ef2d944a9b31b2cc7bdc4",
   "block_height":277316,
   "block_index":64,
   "hash":"0627052b6f28912f2703066a912ea577f2ce4da4caa5a5fbd8a57286c345c2f2",
   "addresses":[
      "1Cdid9KFAaatwczBwBttQcwXYCpvK8h7FK",
      "1GdK9UzpHBzqzX2A9JFP3Di4weBwqgmoQA"
   ],
   "total":9950000,
   "fees":50000,
   "size":258,
   "vsize":258,
   "preference":"high",
   "confirmed":"2013-12-27T23:11:54Z",
   "received":"2013-12-27T23:11:54Z",
   "ver":1,
   "double_spend":false,
   "vin_sz":1,
   "vout_sz":2,
   "confirmations":398396,
   "confidence":1,
   "inputs":[
      {
         "prev_hash":"7957a35fe64f80d234d76d83a2a8f1a0d8149a41d81de548f0a65a8a999f6f18",
         "output_index":0,
         "script":"483045022100884d142d86652a3f47ba4746ec719bbfbd040a570b1deccbb6498c75c4ae24cb02204b9f039ff08df09cbe9f6addac960298cad530a863ea8f53982c09db8f6e381301410484ecc0d46f1918b30928fa0e4ed99f16a0fb4fde0735e7ade8416ab9fe423cc5412336376789d172787ec3457eee41c04f4938de5cc17b4a10fa336a8d752adf",
         "output_value":10000000,
         "sequence":4294967295,
         "addresses":[
            "1Cdid9KFAaatwczBwBttQcwXYCpvK8h7FK"
         ],
         "script_type":"pay-to-pubkey-hash",
         "age":277298
      }
   ],
   "outputs":[
      {
         "value":1500000,
         "script":"76a914ab68025513c3dbd2f7b92a94e0581f5d50f654e788ac",
         "addresses":[
            "1GdK9UzpHBzqzX2A9JFP3Di4weBwqgmoQA"
         ],
         "script_type":"pay-to-pubkey-hash"
      },
      {
         "value":8450000,
         "script":"76a9147f9b1a7fb68d60c536c2fd8aeaa53a8f3cc025a888ac",
         "addresses":[
            "1Cdid9KFAaatwczBwBttQcwXYCpvK8h7FK"
         ],
         "script_type":"pay-to-pubkey-hash"
      }
   ]
}
```

完整地打印出来，它看起来令人生畏，但请记住，我们只关心两件事:

1.  `prev_hash`字段告诉你汇款人从哪里得到的钱，以及
2.  `addresses`字段告诉你发件人是谁。

# 第二步:从 API 响应中获取以前的事务散列

我们需要能够回溯公共账目。因此，对于每笔交易，我们需要获得所有“以前的交易”散列。

## 之前的交易是什么来着？

当 Alice 向 Bob 付款时，“以前的交易”是 Alice 从 Chris 处获得 1BTC 的时间，她现在向 Bob 付款。她可能还从 Chris 那里收到了 0.5BTC，从 Diane 那里收到了 0.5BTC，她将这些钱组合起来支付给 Bob，在这种情况下，有多个“以前的交易”

## 那么，我们如何从 API 响应中获得以前的事务散列呢？

在跳过下面的省略号之前，花一分钟自己尝试一下。提示:你需要知道一些关于字典，“for 循环”和列表的知识。

好了，下面是我的代码，用于从 Blockcypher transactions API 响应中获取以前的事务哈希列表。

```
transaction_info = response.json()  # Blockcypher api response
inputs = transaction_info['inputs']
prev_hashes = []
for elt in inputs:
    prev_hashes.append(elt['prev_hash'])print(prev_hashes)
```

请注意，“elt”是“element”的缩写

如果您在将`response`变量分配给 Blockcypher api 响应后运行我的代码，您应该会看到类似下面的内容打印到控制台:

```
['7957a35fe64f80d234d76d83a2a8f1a0d8149a41d81de548f0a65a8a999f6f18']
```

呜哇！你追踪你的比特币到一笔交易！

# 第三步:从 API 响应中获取发送方钱包地址

前面的事务散列很好，但是您想知道发送者是谁。否则，如果你看不到你的钱经过了谁的手，那么追溯公共账本又有什么意义呢？

对于这一步，尝试从 API 响应中获取发送者的钱包地址。提示:您的解决方案应该与步骤 2 中的代码非常相似…

我是这样得到寄信人地址的:

```
transaction_info = response.json()  # Blockcypher api response
inputs = transaction_info['inputs']sender_addresses = []
for elt in inputs:
    sender_addresses += elt['addresses']print(sender_addresses)
```

我这样做的方式与我收集以前的事务散列的方式有一点不同。特别是，我没有使用`append`将比特币发送者的钱包地址添加到我的列表中，而是使用了`+=`。要了解原因，请尝试将其更改为

```
sender_addresses.append(elt['addresses'])
```

再次运行代码，看看会发生什么。这里我不给任何剧透；这会破坏乐趣的。

# 第四步:重复！

下一步是重复前三步，即:

1.  查询 Blockcypher API 以获取有关事务的信息，
2.  从交易信息中获取以前的交易散列
3.  从交易信息中获取发件人地址。

请注意，由于步骤 2 为我们提供了以前的交易哈希，我们可以再次查询 Blockcypher API 以获得这些交易哈希，然后我们可以在理论上无限重复这个过程，直到我们到达比特币生命周期的开始(当它被“铸造”时)。

然而，更有可能的是，我们只能重复我们的查询，直到 Blockcypher 对我们在短时间内使用他们的 API 太多次感到恼火。这就是所谓的“获得率有限”如果你想知道这到底什么时候会发生，答案是[在这里](https://blockcypher.github.io/documentation/#:~:text=BlockCypher%20APIs%20can%20be%20used,up%20to%20600%20requests%2Fhr)。TLDR:不要每秒调用他们的 API 超过 5 次或每小时 600 次。

完成这一步的一种方法是将前面步骤中的代码转储到一个大循环中。我劝你不要那样做。读起来会很痛苦，调试起来更痛苦。相反，尝试为前面的每个步骤编写函数:

1.  查询 Blockcypher API 的函数，
2.  从 Blockcypher API 响应中获取以前的事务哈希的函数，
3.  和另一个从 Blockcypher API 响应中获取发送者地址的函数。

然后你可以在一个循环中调用这些函数(或者，如果你是良性的，另一个函数！)来多次查询 Blockcypher API 并遍历公共分类帐。

再次重申，请尊重 Blockcypher 的[规则](https://blockcypher.github.io/documentation/#:~:text=BlockCypher%20APIs%20can%20be%20used,up%20to%20600%20requests/hr)关于你多久可以免费查询他们的 API。您可能会发现这段代码对于避免每秒查询 API 超过 5 次非常有用:

```
import timedef intentionally_slow_function():
    time.sleep(0.2)  # waits for 0.2 seconds
    ...  # does other stuff
```

还有一个提示:您可能希望使用事务散列的“队列”,您希望每次查询 Blockcypher 的 API 中的一个事务散列。

好了，现在去试试吧！让我们看看你想出了什么:)

好吧，这是我如何完成这一步的，我们遍历公共账本，收集了你的比特币在去往你的路上经过的钱包地址列表。

```
num_transactions_back = 0
addresses_seen = set()
transaction_queue = [EXAMPLE_TRANSACTION_HASH]
while num_transactions_back < 10 and len(transaction_queue) > 0:
    transaction_info = get_transaction_info_blockcypher(transaction_queue.pop(0))
    transaction_queue += get_prev_hashes(transaction_info)
    sender_addresses = get_sender_addresses(transaction_info)
    addresses_seen.update(sender_addresses)
    num_transactions_back += 1
```

我的算法很简单。在循环内部，我:

*   从队列中弹出事务哈希
*   查询块加密程序的 api
*   将发件人地址添加到地址集中
*   将以前的事务哈希添加到队列中，以进行下一步探索

我一出现以下情况就退出循环:

*   我已经查询 Blockcypher 10 次，或者
*   我的队列是空的

我只遍历公共分类帐 10 步，因为我希望在被 Blockcypher 限制速率之前，能够每小时运行几次我的代码。如果我的比特币在一个粗略的人手里经过了超过 10 个人，我想我不会介意。(如果我对此不满意，我可以随时向 Blockcypher 付费，以获得深入挖掘的能力，或者自己下载公共账本。)

如果你想知道这些神奇的函数像`get_transaction_info_blockcypher`、`get_prev_hashes`和`get_sender_addresses`来自哪里，问得好！我在前面的代码中定义了它们，并将它们的实现留给读者作为练习:)。(通过阅读上面的步骤 1、2 和 3，拼凑起来应该很容易。)

# 第五步:检查付款人是否粗略

最后一步是使用你的钱经过的钱包地址的集合(或列表),并将其与你希望不包括在该集合中的某个钱包的值进行比较。

因为在这个例子中我们只后退了十步，所以理论上你可以用视觉扫描来完成。但是，为了便于讨论，让我们假设你的比特币在到达你的钱包之前经过了许多钱包，而你想要对照已知的“坏钱包”地址检查每一个钱包。您可以使用`1HQ3Go3ggs8pFnXuHVHRytPCq5fGG8Hbhx`作为该步骤的错误地址。(据说是与丝绸之路有关的[。)](https://www.coindesk.com/nearly-1b-in-bitcoin-moves-from-wallet-linked-to-silk-road)

如果你已经走了这么远，这一步应该很简单。来吧，试试看！

```
SILK_ROAD_ADDRESS = '1HQ3Go3ggs8pFnXuHVHRytPCq5fGG8Hbhx'funds_from_silk_road = SILK_ROAD_ADDRESS in addresses_seen
print(f'You do {"" if funds_from_silk_road else "not (as far as we know) "}have funds from silk road!')
```

好的，我在这里使用了一个" [f-string](https://realpython.com/python-f-strings/) "和一个[条件表达式](https://docs.python.org/3/reference/expressions.html#conditional-expressions)。但是这个想法很简单:我的程序检查一个字符串是否在一组字符串中，并使用它来决定输出结果。

# 刚刚发生了什么？

在这个项目中，我们使用了一些基本的 Python 编程概念来做一些很酷的事情:检查我们收到的一些比特币是否与丝绸之路有关(至少，在最近的几次交易中)。

如果你喜欢这个项目，并且希望借此机会学习更多关于编码和/或加密货币的知识，我认为有几种方法可以扩展它，这可能会很有趣:

*   给定一对钱包地址，确定双方是否曾经相互交易(通过那些特定的钱包地址)。
*   给定一对钱包地址，确定钱包之间有多少个“分离度”。(哦看，我们有同一个比特币曾祖父母！你在 2018 年也卖过 craigslist 的商品给 Tony 吗？)
*   绘制一张比特币交易费用“漏桶”图，从你的比特币到达你的账户开始，追溯到最近 10 次左右的交易。

我敢肯定，这方面还有更多我没有想到的好玩的项目——如果你想到了，请评论并告诉我！

此外，如果你理解本文的任何部分有困难，请让我知道，这样我可以重新思考，修改，并为其他学习者改进它。

编码快乐！