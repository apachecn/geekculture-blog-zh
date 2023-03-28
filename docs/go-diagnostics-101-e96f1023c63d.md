# 开始诊断 101

> 原文：<https://medium.com/geekculture/go-diagnostics-101-e96f1023c63d?source=collection_archive---------1----------------------->

## 使用 delve、tracing 和 pprof 来诊断您的 Go 代码

我们这些地鼠肯定已经习惯了 Go 的两张脸。“天使”脸:它很棒，优雅，易于书写。一个用 Java 花你一天时间的服务，用 Go 可能只花你一个小时，甚至包括测试。“魔鬼”脸:当一只虫子突然跳到路上时，事情就变成了困难模式。Go 错误消息难以理解；Java 支持远程调试，可以吗？Java 可以查看 GC 日志，把文件转储到本地，Go 呢？