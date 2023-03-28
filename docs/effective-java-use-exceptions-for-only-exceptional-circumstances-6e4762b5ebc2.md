# 有效的 Java:只在特殊情况下使用异常

> 原文：<https://medium.com/geekculture/effective-java-use-exceptions-for-only-exceptional-circumstances-6e4762b5ebc2?source=collection_archive---------31----------------------->

![](img/4526c743ba4f7407813a635076164092.png)

Photo by [Sarah Kilian](https://unsplash.com/@rojekilian?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在您编码生涯中的某个时刻，您可能会非常不幸地遇到类似如下的代码:

```
try {
  int i = 0;
  while (true) {
    items[i++].process();
  }
} catch (ArrayIndexOutOfBoundsException ignored) {
}
```

很可能这段代码所做的事情不会立即引起你的注意，尤其是与…