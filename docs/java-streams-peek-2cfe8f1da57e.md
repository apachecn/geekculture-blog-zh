# Java 流:Peek

> 原文：<https://medium.com/geekculture/java-streams-peek-2cfe8f1da57e?source=collection_archive---------15----------------------->

![](img/bb256241cc5df05db2e45bfe3e344f49.png)

Photo by [Sprocket2cog](https://commons.wikimedia.org/w/index.php?title=User:Sprocket2cog&action=edit&redlink=1) from [Wikimedia Commons](https://commons.wikimedia.org/wiki/File:Urban_stream_in_park.jpg)

## 循环时对每个流元素执行操作

如果您需要对每个**流**项目执行一个操作，然后继续其他**中间操作**，您将需要这样的东西:

*   对**流**项执行操作
*   是一个中间操作并返回一个继续工作的**流**