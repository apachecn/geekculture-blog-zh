# Kubernetes 自动缩放:你需要知道的事情！

> 原文：<https://medium.com/geekculture/kubernetes-autoscaling-things-you-need-to-know-4621b7473514?source=collection_archive---------17----------------------->

你不知道你的云软件服务什么时候会面临用户负载。因此，您应该总是在云集群中准备您的节点和单元。但是，总是打开集群会浪费资源。还有，效率会低。

Kubernetes 的自动伸缩特性将很好地利用集群节点和 pod。它会将负载调整到所需的资源。当集群服务上的流量负载很小时，Kubernetes autoscaling 将打开几个节点。当有更多的流量时，它将使用更多的节点来满足…