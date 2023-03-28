# 如何在 Java 中实现对象的深度复制

> 原文：<https://medium.com/geekculture/how-to-implement-deep-copy-of-an-object-in-java-609d739d6540?source=collection_archive---------2----------------------->

## 方案

想象一下，你需要复制一个对象，你想保持旧的不变。在这种情况下，原始对象中的所有引用应该被构造为不同的引用，这被称为深度复制技术。

# 例子

假设我们有一个名为 *OrderDetails* 的对象，它包含字段 *uerId* 、 *orderDate* 和 *itemId* :

```
public class OrderDetails {

    private String userId;
    private String orderDate;
    private String itemId…
```