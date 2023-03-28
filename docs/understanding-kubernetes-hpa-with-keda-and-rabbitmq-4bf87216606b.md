# 通过 KEDA 和 RabbitMQ 了解 Kubernetes HPA

> 原文：<https://medium.com/geekculture/understanding-kubernetes-hpa-with-keda-and-rabbitmq-4bf87216606b?source=collection_archive---------3----------------------->

真实世界的 Kubernetes 无状态应用程序经过精心设计，将代码与其他组件(如数据库或队列)分离开来，这使得扩展或缩减代码组件(或所谓的无状态应用程序)变得更加容易。有不同的场景需要扩展应用程序，这取决于我们业务需求的性质。一个场景可以是在 Kubernetes 集群之外的 RabbitMQ 服务器中处理消息的应用程序，队列中可能有数千或数百万条消息，必须尽快处理…