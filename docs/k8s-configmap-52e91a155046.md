# K8s —配置图

> 原文：<https://medium.com/geekculture/k8s-configmap-52e91a155046?source=collection_archive---------4----------------------->

## 每天一点 K8s 知识！

![](img/e117dac5685b90a9f4e7615d2ff50b17.png)

K8s `Secrets`可以为 Pod 提供密码、令牌、私钥等敏感数据；对于一些非敏感数据，比如应用配置信息，可以使用`ConfigMap`。

# 配置图

`ConfigMap`是一个 API 对象，用于以键值对的形式存储非机密数据。豆荚可以消耗`ConfigMaps`作为…