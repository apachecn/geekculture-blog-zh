# K8s —库伯莱

> 原文：<https://medium.com/geekculture/k8s-kubelet-621fe55c3c7?source=collection_archive---------2----------------------->

## 每天一点 K8s 知识！

![](img/5494aced8bca1a189d1a22dee3b9336f.png)

`kubelet`是在每个节点上运行的主要“节点代理”。它可以使用以下之一向`apiserver`注册节点:主机名；覆盖主机名的标志；或者云提供商的特定逻辑。

`kubelet`默认监听 10250 端口，接收并执行 Master 发送的指令，管理 Pod 和 Pod 中的容器。