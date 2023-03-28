# Java 流:无匹配

> 原文：<https://medium.com/geekculture/java-streams-nonematch-9937034f5a39?source=collection_archive---------8----------------------->

![](img/c1e5a91639511712eefbc3351615b5fe.png)

What a Beautiful Stream! Photo by [Sprocket2cog](https://commons.wikimedia.org/w/index.php?title=User:Sprocket2cog&action=edit&redlink=1) from [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Urban_stream_in_park.jpg)

## 在流中搜索

在列表或**流**中查找内容是一件容易的事情。

**Java 8 流 API** 授予我们几个**布尔返回**方法，这些方法做得恰到好处。它们都需要一个**谓词**。

## 不匹配

如果**流**项的 **none** 满足**谓词**，则`**noneMatch**()`方法返回 true。可能没有必要询问…