# 春季活动驾驶如何帮助我的工作

> 原文：<https://medium.com/geekculture/how-spring-event-driving-helps-in-my-work-e54c8257b61c?source=collection_archive---------6----------------------->

在一些开发场景中，我们需要在某些事件触发时将业务流程从一种状态推到另一种状态。

在我的工作中，Spring 事件驱动结构有助于解决这种情况。让我们看看下面的例子。

在这个例子中，我们的用户将申请一个文档。

基础 *ProcessEvent* 有唯一的 *processId* 和可定制的对象 *processData* ，存储应用进程中的相关数据。