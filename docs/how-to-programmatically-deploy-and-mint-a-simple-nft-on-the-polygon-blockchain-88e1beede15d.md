# 如何在多边形区块链上以编程方式部署和创建一个简单的 NFT

> 原文：<https://medium.com/geekculture/how-to-programmatically-deploy-and-mint-a-simple-nft-on-the-polygon-blockchain-88e1beede15d?source=collection_archive---------0----------------------->

*快速指南，在没有 NFT 市场的情况下部署您的第一个 NFT，同时仍然能够在 OpenSea 上进行交易。*

![](img/93f271841f3572fbb8119634128754bb.png)

Photo by [Austin Distel](https://unsplash.com/@austindistel?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/ethereum?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在这个时候，区块链是一个有趣的软件。这是一个非常热门的话题，开发者正在慢慢上升到顶端。以太坊，最大的智能合约区块链正在迅速崛起。然而问题是，**它的汽油费也随之上涨**。部署一份简单的 NFT 智能合同将花费我 1300 美元。

这就是为什么我们要使用[多边形](https://polygon.technology/)的区块链。这是一个以太坊兼容的区块链，许多项目采用它作为第二选择。**它的油费低得多**，但功能不变。我们还将使用[安全帽](https://hardhat.org/)。用 JavaScript 连接区块链的一种方式。

# 项目设置

我们将在 Polygon Testnet(孟买)上部署和铸造我们的 NFT。我们将使用 Polygon，因为它的天然气费用较低，并且仍然能够与 OpenSea 等平台连接。

## 设置您的钱包

首先，您需要设置元掩码，获取一些 testnet MATIC 并使其出现在您的钱包中。你可以按照我写的另一篇文章来实现这一点。

[](/codex/setting-up-metamask-for-polygon-blockchain-development-af058d0fab2e) [## 为多边形区块链开发设置元掩码

### 关于设置和获得孟买测试网自动令牌的完整指南。

medium.com](/codex/setting-up-metamask-for-polygon-blockchain-development-af058d0fab2e) 

创建钱包并设置好多边形运行所需的一切后，您可以继续。

## 设置项目

我们将使用 [Hardhat](https://hardhat.org/) 部署我们的智能合约。Hardhat 是一个以太坊开发环境，但谢天谢地，它对多边形和其他链也很有效。我们还将使用 NPM 来设置一切。

```
npm install --save-dev hardhat
npx hardhat
```

第二个命令将提示您一些选项。在这种情况下，您可以选择默认选项。你可以在官方文件中了解更多。

您现在应该有一个目录，其中包含一些文件和文件夹，如合同、脚本和测试。

# 制定明智的合同

我们将使用 OpenZeppelin 的契约库快速创建一个 ERC721 智能契约，它符合 ERC721 标准中指定的规范，这是 NFTs 的默认规范。

```
npm install @openzeppelin/contracts
```

接下来，我们将在 Contracts 文件夹中使用这些有用的工具创建我们自己的合同。您可以将下面要点中的代码复制并粘贴到一个名为`MyToken.sol`的文件中。代码是用 [OpenZeppelin 的令牌向导](https://docs.openzeppelin.com/contracts/4.x/wizard)生成的，这是一个制作简单契约的有用工具。

该合同有能力由合同所有者铸造。它会使用`Counters`自动跟踪`TokenId`，通过使用`Enumerable`，我们可以很容易地跟踪有多少代币以及它们属于谁。底部的两个功能是`Enumerable`所需的简单覆盖。

阅读我们在 [OpenZeppelin](https://docs.openzeppelin.com/contracts/4.x/api/token/erc721#ERC721) 上导入的契约的能力会非常有用。只需阅读函数和它们的作用。如果你需要更多关于坚固的知识，我建议去做隐僵尸教程。

# 部署

我们可以创建一个简单的脚本，调用它将我们的脚本部署到我们选择的网络中。多边形的孟买测试网。为此，我们还需要在 Hardhat 配置中添加一些小选项。

在脚本文件夹中，创建一个名为`deploy-script.js`的新文件，并向其中添加以下代码。

此外，修改`hardhat.config.js`以包含以下代码。

私钥应该是元掩码钱包中的私钥。如果您的钱包中有 testnet MATIC 令牌(私钥来自该令牌),那么我们现在已经拥有了部署契约所需的一切，方法是运行:

```
npx hardhat run scripts/deploy-script.js --network matic_testnet
```

在控制台中复制它将返回的地址，你甚至可以在孟买多边形扫描网站上查看它。

# 铸造

如果契约被部署，您应该能够以多种方式调用`mint`函数。你可以用 Web3 连接你的钱包，让一个按钮调用`mint`，但是我们也可以用另一个脚本来完成。

你可以用它打电话

```
npx hardhat run scripts/mint-script.js --network matic_testnet
```

你的第一个 NFT 现在应该铸造出来了，你应该可以在 OpenSea 上看到它们。

然而，这个 NFT 没有 URI，没有图像，也没有元数据。这是另一篇文章的内容。

# 结论

如果你想制造大量的 NFT，你可以使用一个在你的 cmd 中执行命令的库来循环运行`npx`命令。让我在推特上知道你创造了什么样的 NFT 项目！

非常感谢您的阅读，祝您度过美好的一天。