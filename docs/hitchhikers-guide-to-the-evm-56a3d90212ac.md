# EVM 搭便车旅行指南

> 原文：<https://medium.com/geekculture/hitchhikers-guide-to-the-evm-56a3d90212ac?source=collection_archive---------21----------------------->

## 通过优化存储进行气体高尔夫

![](img/450faae806efa102af28be35b8e0f354.png)

*本文是在*[*SmartCon # 1*](https://www.smartcontractsummit.io/)*上的一次演讲的文字整理。*

Gas golfing 是优化智能合同现有功能的过程，而不实际改变其功能。具体来说，优化合同对 ***存储*** 的使用是一些最大的优势所在。话虽如此……