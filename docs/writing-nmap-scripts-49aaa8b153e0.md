# 编写 Nmap 脚本

> 原文：<https://medium.com/geekculture/writing-nmap-scripts-49aaa8b153e0?source=collection_archive---------8----------------------->

## 通过编写自己的扫描方法从 Nmap 获得更多

![](img/eb6974dbac9363dfcdb0a28f5e33631b.png)

H ello，🌎！在这篇博文中，我介绍了如何编写 Nmap 脚本引擎脚本( **NSE** )脚本。Nmap 附带了数百个脚本。很多这样的脚本实际上都是在你用常用的`-sC`标志运行 Nmap 的时候。这些脚本做的事情，如执行基本的 HTTP 枚举，尝试连接和列表…