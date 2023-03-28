# 如何在 Azure 管道中使用密钥库机密

> 原文：<https://medium.com/geekculture/how-to-use-key-vault-secrets-in-azure-pipelines-658198f2eea6?source=collection_archive---------4----------------------->

## DevOps 良好实践

## 在 Azure Pipelines 中访问密钥库机密的不同方式。

![](img/71d8f1df1a5c11e35345fb92384d8812.png)

Photo by [Kristina Flour](https://unsplash.com/@tinaflour?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

使用密钥库机密来存储敏感数据对于确保数据安全至关重要。通过使用秘密，我们可以控制谁有权查看和操作存储在其中的数据，确保只有正确的人拥有…