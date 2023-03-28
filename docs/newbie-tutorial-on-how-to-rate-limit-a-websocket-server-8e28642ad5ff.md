# 关于如何限制 WebSocket 服务器速率的新手教程

> 原文：<https://medium.com/geekculture/newbie-tutorial-on-how-to-rate-limit-a-websocket-server-8e28642ad5ff?source=collection_archive---------23----------------------->

## 我在 Ubuntu 服务器上用 ufw 加固了一个安全的 WebSocket 服务器

![](img/db5b4c138c2ce8cf50981351eee4e513.png)

速率限制就是实施安全策略来控制或限制特定服务接收的网络流量。其主要目的之一是减轻网络罪犯的 [DoS(拒绝服务)攻击](https://en.wikipedia.org/wiki/Denial-of-service_attack)。这是任何……