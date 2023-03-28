# 使用自定义插件扩展 Salesforce CLI

> 原文：<https://medium.com/geekculture/extending-the-salesforce-cli-with-a-custom-plugin-29c0438a66e0?source=collection_archive---------7----------------------->

随着越来越多的服务迁移到云中，DevOps 方法也在不断发展，越来越多的开发人员开始习惯在终端上工作。传统的 CLI 命令如`grep`和`cat`是众所周知的实现小目标的工具，而更复杂的任务需要更强大的工具。

如今，CLI 程序带来了更丰富的交互体验。一个这样的程序是 [Salesforce CLI](https://developer.salesforce.com/tools/sfdxcli) ，Salesforce DX 的命令行界面。这是一个 CLI，在构建应用程序时，它有助于简化常见操作…