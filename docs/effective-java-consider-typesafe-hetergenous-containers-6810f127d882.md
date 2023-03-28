# 有效的 Java！考虑类型安全的异质容器

> 原文：<https://medium.com/geekculture/effective-java-consider-typesafe-hetergenous-containers-6810f127d882?source=collection_archive---------63----------------------->

泛型最常见的用例是像`List`和`Map`这样的集合，以及像`ThreadLocal`和`AtomicReference`这样的单元素容器。在集合情况和单元素对象中，都有一个非常有限的类型列表。这在很多情况下对我们很有用，但是有时我们需要额外的灵活性。我们可能有一个用例，比如以类型安全的方式映射数据库中的一行。在这些情况下，我们必须使用另一种技术。这个技术是将数据的*键*参数化，而不是将*容器*完全参数化。那个…