# 举例说明:Dijkstra 算法

> 原文：<https://medium.com/geekculture/explain-by-example-dijkstras-algorithm-d0a3d464314d?source=collection_archive---------39----------------------->

最近，我接到一个编码挑战，这个挑战是，给定一些 OpenStreetMap 数据，找出两个数据点之间的最短距离。我立刻想到，“啊哈！这是一个 [A*搜索](https://en.wikipedia.org/wiki/A*_search_algorithm)的问题。”

然后我读了问题陈述，发现 A*可能过度设计了问题，此外，在我的教授讨论 A*搜索算法的启发式方面的所有课上，我不是都睡着了吗？

不管怎样，既然我在 A*搜索算法讲座上睡着了，那就让我们回到…