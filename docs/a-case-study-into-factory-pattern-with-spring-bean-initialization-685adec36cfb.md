# 带有 Spring Bean 初始化的工厂模式案例研究

> 原文：<https://medium.com/geekculture/a-case-study-into-factory-pattern-with-spring-bean-initialization-685adec36cfb?source=collection_archive---------12----------------------->

在 FinTech 世界中，我们经常需要交易来帮助我们实现原子目的。

例如，我们需要

*   将付款请求插入表中，
*   调用其他系统来处理请求，
*   并更新付款人的账户历史。

这 3 个动作可以包装在一个事务中。如果一个行动失败了，其余的都会失败。