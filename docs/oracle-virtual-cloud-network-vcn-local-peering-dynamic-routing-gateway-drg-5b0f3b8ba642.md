# Oracle 虚拟云网络(VCN)本地对等—动态路由网关(DRG)

> 原文：<https://medium.com/geekculture/oracle-virtual-cloud-network-vcn-local-peering-dynamic-routing-gateway-drg-5b0f3b8ba642?source=collection_archive---------15----------------------->

虚拟云网络(VCN)对等是虚拟云网络(vcn)之间的网络连接。任一 VCN 中的实例可以相互通信，就好像它们在同一个网络中一样。您可以在同一个区域内创建 VCN 对等，称为本地对等，也可以在 OCI 区域间创建远程对等。

## 对等网络上的流量控制

*   路由表—您可以使用 VCNs 中的路由表来控制对等连接上的数据包流。