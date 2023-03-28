# 使用 C# API 版本控制清理架构

> 原文：<https://medium.com/geekculture/clean-architecture-using-c-api-versioning-128559de808f?source=collection_archive---------0----------------------->

# **什么是 API 版本控制？**

API 版本化是一种实践，即同一事物有多个端点，但有不同的返回对象、不同的入口对象，甚至过程的各种升级。这看起来很复杂，但相对简单。例如，您有一个返回 Todo 信息的 REST 端点。开始时，您的 Todo 只有一个描述，但后来，Todo 也有一个类别，您需要区分每个修改。您可以更改端点，以便可以用…获得新的 Todo