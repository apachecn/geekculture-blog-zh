# 初学者指南:提取转换负载(ETL)行动手册——增量负载设计模式第 3 部分

> 原文：<https://medium.com/geekculture/beginners-guide-extract-transform-load-etl-playbook-incremental-load-design-pattern-part-3-5b08c428c1dd?source=collection_archive---------24----------------------->

*本文的目标读者是对理解一些数据工程原理感兴趣的 IT 爱好者和初级数据工程师*

在之前的帖子中(这里读[](https://afroinfotech.medium.com/beginners-guide-extract-transform-load-etl-playbook-incremental-load-design-pattern-part2-816d2f9e921f)*)，我详细描述了一个增量负载设计模式和解决方案。在本文中，我将探索在目标表中插入和更新数据的方法，并讨论使用变更数据捕获(CDC)和流变更数据捕获的近实时增量数据提取过程*