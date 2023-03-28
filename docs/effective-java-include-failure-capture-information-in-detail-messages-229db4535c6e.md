# 有效的 Java:在详细消息中包含失败捕获信息

> 原文：<https://medium.com/geekculture/effective-java-include-failure-capture-information-in-detail-messages-229db4535c6e?source=collection_archive---------25----------------------->

![](img/977f55bca86bb49ae503b37274529db9.png)

Photo by [Scott Van Hoy](https://unsplash.com/@svanhoy?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

通常，当我们分析某个代码的失败时，我们只剩下遗留的日志。鉴于这种情况，我们想给自己最大的成功机会。当程序失败时，系统将自动输出异常的堆栈跟踪，其中包括从异常的`toString`方法返回的值。这个…