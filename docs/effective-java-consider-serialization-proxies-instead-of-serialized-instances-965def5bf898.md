# 有效的 Java:考虑序列化代理而不是序列化实例

> 原文：<https://medium.com/geekculture/effective-java-consider-serialization-proxies-instead-of-serialized-instances-965def5bf898?source=collection_archive---------14----------------------->

![](img/6ba6d5fba015070917cdbbc2d65fe083.png)

Photo by [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

纵观[所有](https://blog.devgenius.io/effective-java-for-instance-control-prefer-enum-types-to-readresolve-4c30c71b765e)[最近](/geekculture/effective-java-consider-using-a-custom-serialized-form-b3fa56d0edc8) [项](/codex/effective-java-implement-serializable-with-great-caution-df123eb51cdf) [正如](https://blog.devgenius.io/effective-java-prefer-alternatives-to-java-serialization-3cf14eee190)我们已经讨论了 Java 序列化，我们已经讨论了随之而来的许多挑战。虽然表面上看起来实现起来很简单，但实际上远非如此。由于序列化框架`Serializable`提供了有效的隐藏构造函数，代码对许多潜在的问题是开放的…