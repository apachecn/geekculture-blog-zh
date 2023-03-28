# Istio、eBPF 和 RSocket Broker:深入服务网格

> 原文：<https://medium.com/geekculture/istio-ebpf-and-rsocket-broker-a-deep-dive-into-service-mesh-7ec4871d50bb?source=collection_archive---------3----------------------->

# 介绍

在微服务时代，复杂的应用程序被分解成多个单元，**组件化、协作和连接，**服务倾向于承担越来越多的业务职责，这使得服务管理变得前所未有的困难。仅仅依靠微服务框架级治理是不够的，应该构建一个高维度的深度治理系统。