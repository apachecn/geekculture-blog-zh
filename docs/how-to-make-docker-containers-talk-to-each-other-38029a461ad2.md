# 如何让 Docker 容器相互对话

> 原文：<https://medium.com/geekculture/how-to-make-docker-containers-talk-to-each-other-38029a461ad2?source=collection_archive---------1----------------------->

## 通过使用桥接网络

![](img/569d013a1c78226297388bf4353db4b5.png)

当您想让两个 Docker 容器在同一个服务器上相互通信时，一个很好的例子是 REST API 和 MySQL 数据库，但它们都在自己的存储库和容器中，以保持关注点的分离。

如前一篇文章所解释的，[我用 Docker CMD](/geekculture/i-replaced-supervisor-with-the-docker-cmd-instruction-18b1d3b5ebd1) 替换了 Supervisor