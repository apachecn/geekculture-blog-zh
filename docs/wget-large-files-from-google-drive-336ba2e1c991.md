# 2023 年使用终端从 google drive 获取大文件[更新]

> 原文：<https://medium.com/geekculture/wget-large-files-from-google-drive-336ba2e1c991?source=collection_archive---------9----------------------->

![](img/06d8508618d7a4c0fac3ba147b709e07.png)

最近，我不得不从 google drive 下载一些大于 5GB 的文件到一个只允许通过终端通信的远程服务器上。因此，由于没有合适的指南，我通过 StackOverflow 搜索了一些类似的问题。

我发现的解决方案是，我们可以使用 wget 和下面的命令来获取更大的文件，这些文件位于…