# 使用 Docker + Grafana + Prometheus + Node 监控您的系统

> 原文：<https://medium.com/geekculture/monitoring-your-system-with-docker-grafana-prometheus-node-d7fae11416f3?source=collection_archive---------0----------------------->

![](img/b4414dcfefa4f7a06f3318f13f1bccae.png)

在本指南中，我们将学习如何将 Grafana、Prometheus 和 Node Exporter 作为 Docker 容器运行，这些容器由 [Docker Compose](https://docs.docker.com/compose/) 管理。您将把相关的主机目录挂载到节点导出器、Prometheus 和 Grafana 容器中，配置 Prometheus 来收集节点导出器指标。