# 使用 Docker + Duplicati 进行备份

> 原文：<https://medium.com/geekculture/backups-with-docker-duplicati-533da56b7bb9?source=collection_archive---------1----------------------->

![](img/040b7356ba9a4b1f9c268ab2fa19a403.png)

## 什么是 Duplicati？

Duplicati 是一个开源备份客户端，可以运行加密的增量备份到本地存储或异地，支持许多不同的文件传输协议。

## 许多后端

Duplicati 支持标准协议，如 FTP、SSH、WebDAV，以及流行的服务，如: [Backblaze B2](https://www.backblaze.com/blog/duplicati-backups-cloud-storage/) 、 [Tardigrade](https://tardigrade.io/) 、微软…