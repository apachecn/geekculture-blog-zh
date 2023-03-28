# 用 CodeQL 检测 Jackson 反序列化漏洞

> 原文：<https://medium.com/geekculture/detecting-jackson-deserialization-vulnerabilities-with-codeql-8ec6353c5cc6?source=collection_archive---------62----------------------->

## 如何发现、修复并防止它们在未来发生

如果您使用 Jackson Databind 库并运行安全扫描程序，您可能会收到很多关于反序列化漏洞的警报。过去，几乎每个月都会出现一个新的 CVE，因为有人发现了一个新的反序列化小工具，可以用来利用应用程序。幸运的是，该项目不再为新的反序列化小工具分配 CVE。这是有意义的，因为一个应用程序…