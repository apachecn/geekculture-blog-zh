# Java 流:任何匹配

> 原文：<https://medium.com/geekculture/java-streams-anymatch-6a1a22df632e?source=collection_archive---------12----------------------->

![](img/c1e5a91639511712eefbc3351615b5fe.png)

What a Beautiful Stream! Photo by [Sprocket2cog](https://commons.wikimedia.org/w/index.php?title=User:Sprocket2cog&action=edit&redlink=1) from [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Urban_stream_in_park.jpg)

## 在流中搜索

在列表或**流**中查找东西是一件容易的工作。

**Java 8 Stream API** 给了我们一些**布尔返回**的方法，这些方法做得恰到好处。它们都需要一个**谓词**。

## 任何匹配

如果**流**的至少一个 项满足**谓词**，则`**anyMatch**()`方法返回真。可能没必要询问…