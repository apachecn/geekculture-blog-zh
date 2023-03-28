# 在 Oracle 云上配置私有负载平衡器

> 原文：<https://medium.com/geekculture/configure-private-load-balancer-on-oracle-cloud-77122cf49c33?source=collection_archive---------10----------------------->

Oracle 云基础设施(OCI)提供负载平衡功能，使客户能够跨服务器群分发 web 请求，或者跨容错域、可用性域或区域自动路由流量，从而为任何应用程序或数据源提供高可用性和容错能力。

OCI **Flex** 负载平衡器自动在多个目标之间分配传入的应用流量，如 oracle 计算实例、IP 地址等。OCI 负载平衡器包含两种服务