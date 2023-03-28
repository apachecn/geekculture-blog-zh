# Java 流:限制和跳过

> 原文：<https://medium.com/geekculture/java-streams-limit-and-skip-24c9732c60a?source=collection_archive---------20----------------------->

![](img/bb256241cc5df05db2e45bfe3e344f49.png)

Photo by [Sprocket2cog](https://commons.wikimedia.org/w/index.php?title=User:Sprocket2cog&action=edit&redlink=1) from [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Urban_stream_in_park.jpg)

## 截断流

如果你去

```
user.**stream**().**count**();
```

您将在**流**/列表中检索`long`值的项目数。

处理**流**大小可能会有用，特别是有两个**中间操作**，我发现它们很容易使用和方便。

## 限制