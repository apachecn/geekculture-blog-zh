# 如何:PowerShell 和 Regex

> 原文：<https://medium.com/geekculture/how-to-powershell-and-regex-9f2d3f1a9672?source=collection_archive---------16----------------------->

## 在 PowerShell 脚本中轻松利用正则表达式

![](img/88e33c09eb4c8b1eeb2c2400ec057f85.png)

Regular expressions are always associated with evil by any programmer. Photo from: [What are Evil Regexes?. Regular expressions are cute, but there… | by Nitin Patel | Medium](/@nitinpatel_20236/what-are-evil-regexes-7b21058c747e) [Nitin Patel](https://medium.com/u/fae9d84f67cb?source=post_page-----9f2d3f1a9672--------------------------------)

我将向您展示如何在 PowerShell 命令中轻松使用**正则表达式**。

假设我有一个字符串数组，我对它运行一个 for 循环，并尝试用一个**正则表达式**匹配该字符串: