# 以太坊伦敦硬叉——会有什么变化？

> 原文：<https://medium.com/geekculture/ethereum-london-hard-fork-what-will-change-a20b3461381a?source=collection_archive---------3----------------------->

2021 年 8 月 4 日审查 5 项以太坊改进提案。

![](img/ee976262c0674ec2572f998b58034328.png)

Photo by [Executium](https://unsplash.com/@executium?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/ethereum?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

备受期待的以太坊伦敦硬叉将于 2021 年 8 月 4 日推出[。它包含五个以太坊改进建议(EIP)来改进以太坊网络。这些生态工业园最初是如何提出的，为什么？他们为以太坊网络解决了哪些问题？](https://coinmarketcap.com/currencies/ethereum/)

# 什么是 EIP？

你会经常听到像 EIP-1559 这样的短语。没那么花哨。每个协议号表示以太网提议的变更数量。
社区内的任何人都可以提名 EIP。

是的，任何参与以太坊的人都可以创建一个 EIP 提案。以太坊社区的利益相关者将最终决定是否采用它。

EIP 受到了《比特币改良提案》(BIP)中比特币版本的启发。创建 EIP 是为了记录以太坊网络的改进和变化。

![](img/35091ca69345c45e600b4b11a17408cb.png)

Photo by [Jurica Koletić](https://unsplash.com/@juricakoletic?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/london?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 伦敦福克的 EIP 改进

## [EIP-1559:ETH 1.0 链的费用市场变化。](https://eips.ethereum.org/EIPS/eip-1559)

EIP-1559 试图解决网络费用的不可预测性。当用户进行 ETH 交易时，他们可以在“快速”、“正常”或“慢速”竞价之间进行选择。这类似于投标挖掘节点。

意想不到的后果是，矿商有动力为最高出价服务(“快速”)。导致加密用户为他们的交易支付更高的网络费用。

EIP-1559 引入了交易发生时需要支付的最低费用。这个最低网络费用(基本费用)根据网络处理的交易请求数量而波动。

如果你希望交易更快，你可以通过给小费来激励矿工。矿工只能得到交易的小费。因为基本费用被烧回网络。这种“烧钱机制”也充当了 ETH 的通缩机制(与比特币不同，ETH 没有供应上限)。

EIP-1559 会降低汽油费吗？看情况。但这肯定会让 ETH 的天然气费用变得更加可预测。

## [EIP-3198: BASEFEE 操作码](https://eips.ethereum.org/EIPS/eip-3198)

与 EIP-1559 的改进直接相关。BASEFEE 操作码将当前块的基本费用值返回给 EVMs(运行以太坊客户端的计算机)。这使得智能合约或 DApps 可以立即获得基本费用的价值。EIP-3198 旨在让 DApps 和智能合约改善他们的服务。

## [EIP-3529:退款减少](https://eips.ethereum.org/EIPS/eip-3529)

EIP-3529 取消自毁的气体退款，减少商店的气体退款。

最初，引入了气体退还存储和自毁。这是开发人员编写有效利用存储和减少冗余代码的应用程序的动机。当可变存储减少时，它退还气费。

## [EIP-3541:拒绝带有 0xEF 字节的新合同](https://eips.ethereum.org/EIPS/eip-3541)

具有 0xEF 字节的现有智能合约将保留，但是具有 0xEF 字节的新智能合约将被拒绝。
将新的契约分离成一个新的字节序列。

EIP-3541 升级是因为 EVM 对象格式(EVMOF)的实现。为了防止以太坊客户端混淆不在 EVMOF 中的以前的智能合约和 EVMOF 格式的新智能合约。

如果 EVMOF 提案没有通过。EIP-3541 可以在需要它的未来更新中实现。

## [EIP-3554:难度炸弹延迟至 2021 年 12 月](https://eips.ethereum.org/EIPS/eip-3554)

推迟采矿难度增加到 2021 年 12 月。这个难度升级是为了降低以太坊采矿的吸引力。鼓励网络从 ETH 1.0 工作证明转移到 ETH 2.0 利益证明机制。

EIP-3554 动机是在 ETH 1.0 和 ETH 2.0 合并后增加难度。合并计划于 2021 年 10 月在上海升级。

# 关键要点

*   EIP-1559 引入了以太通货紧缩机制，使网络费用更可预测。
*   EIP-3198 让以太坊客户端计算机获得基本费用的当前值。
*   EIP-3529 减少了气体退款系统的漏洞，并减少了堵塞网络的垃圾数据。
*   EIP-3541 要求新的智能合约是不同于现有 0xEF 的机器代码。
*   EIP-3554 将采矿难度炸弹推迟到 2021 年 12 月，以配合 ETH 1.0 和 ETH 2.0 的合并

想更多地了解区块链吗？我在[*comprehend.substack.com*](https://comprehend.substack.com)分享一下区块链的项目或者区块链的基础知识