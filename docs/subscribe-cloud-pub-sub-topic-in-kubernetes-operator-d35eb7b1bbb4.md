# 在 Kubernetes Operator 中订阅云发布/订阅主题

> 原文：<https://medium.com/geekculture/subscribe-cloud-pub-sub-topic-in-kubernetes-operator-d35eb7b1bbb4?source=collection_archive---------12----------------------->

![](img/7155c82d198d8fd5255a11f91848142f.png)

from unsplash, [@jasonhogan](https://unsplash.com/photos/YyFwUKzv5FM)

上周，我在我们的 operator 项目中处理一个订阅 PubSubTopic 的任务，我认为这值得写下来。它给我的感觉就像到达山顶一样轻松。前方道路崎岖不平，但你会挺过去。

路上有四块“大石头”，我一次打死它们就顺利上路了。它们是四个“如何”