# 使用 HoneyComb +跟踪您的应用程序。网络 6

> 原文：<https://medium.com/geekculture/tracing-your-application-with-honeycomb-net-6-e7392bed273b?source=collection_archive---------10----------------------->

我已经写了关于可观察性、跟踪和日志，你可以在这里看到-> [如何做跟踪。Net application |作者 Bernardo Teixeira |极客文化|媒体](/geekculture/how-to-do-tracing-in-net-application-9961edf66cfb)

但在本文中，我测试的是蜂巢云。测试的是同步、异步和多线程的行为。

蜂巢的一些特征:

*   您的运行代码和基础设施可以被检测以发出[事件、日志](https://docs.honeycomb.io/learning-about-observability/events-metrics-logs/)和[跟踪](https://docs.honeycomb.io/getting-data-in/tracing/send-trace-data/)，它们包含您需要调试和故障排除的上下文。[打开遥测或](https://docs.honeycomb.io/getting-data-in/) …