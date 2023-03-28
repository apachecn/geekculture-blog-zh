# 如何使用 Spring Cache 实现更高效的数据访问

> 原文：<https://medium.com/geekculture/how-to-use-spring-cache-for-more-efficient-data-access-5a82aa733b54?source=collection_archive---------3----------------------->

![](img/37f792666a5ca832322c5bc8ae20623f.png)

缓存是近年来提高应用程序性能的一种技术趋势。在本文中，我们将通过一个连续的代码流，详细介绍 Spring Cache 框架下常见的缓存注释，包括 Cacheable、CachePut 和 CacheEvict。

## **导入依赖**

```
<dependency>
    <groupId>org.springframework.boot</groupId>…
```