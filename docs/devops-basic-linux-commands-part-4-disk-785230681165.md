# DevOps —基本 Linux 命令第 4 部分，磁盘

> 原文：<https://medium.com/geekculture/devops-basic-linux-commands-part-4-disk-785230681165?source=collection_archive---------8----------------------->

## 磁盘故障排除

![](img/bdf2b87c124cd6f7c0972bbe5c4823ef.png)

# df

`df`:显示与文件系统相关的总空间和可用空间信息。

常见用法:

```
$ df -a ==> includes pseudo, duplicate and inaccessible fs.
$ df -B 1M ==> scales sizes by 1M before printing them.
$ df -h ==> print sizes in power of 1024
$ df -H ==> print…
```