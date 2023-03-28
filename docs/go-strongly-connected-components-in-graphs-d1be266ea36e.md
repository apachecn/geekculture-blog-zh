# Go:图中的强连通分量

> 原文：<https://medium.com/geekculture/go-strongly-connected-components-in-graphs-d1be266ea36e?source=collection_archive---------25----------------------->

用 Golang 寻找有向图中的 SCC

![](img/98e835adac456ab74c518b877dbb07b3.png)

Photo by [Alina Grubnyak](https://unsplash.com/@alinnnaaaa?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/network?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

想象一下，您必须编写代码来发现社交网络中的群组，这样您就可以建议他们互相关注，或者根据他们的共同兴趣来喜欢某个页面。

网络中的人可以表示为图的顶点，而组可以表示为图的强连通部分。