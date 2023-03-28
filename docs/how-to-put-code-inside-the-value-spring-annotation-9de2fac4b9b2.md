# 如何将代码放入@Value Spring 注释中

> 原文：<https://medium.com/geekculture/how-to-put-code-inside-the-value-spring-annotation-9de2fac4b9b2?source=collection_archive---------18----------------------->

![](img/d44ff1ee640325d957e0c46615101e92.png)

Photo From: [https://www.cleanpng.com/png-spring-framework-software-framework-java-applicati-6290527/](https://www.cleanpng.com/png-spring-framework-software-framework-java-applicati-6290527/)

**@Value** **注释**允许将值注入类字段(以及其他注释注入接口的实现，如`**@Autowired**`)。

例如:

```
**@Value**("hello mama")
private String myInjectedString;
```

在这里，我们将一个**字符串文字**值注入到我们的字段中，我们可以用任何其他类型来完成它。相当…