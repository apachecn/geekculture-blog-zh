# Java 编码技巧—函数式编程

> 原文：<https://medium.com/geekculture/java-coding-tip-functional-programming-1b6278e48efa?source=collection_archive---------2----------------------->

![](img/11d4617ea59afb39cb4a9cbf8505fff1.png)

Photo by [Danial Igdery](https://unsplash.com/@ricaros?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/programming?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 方案

假设我们有一个字符串列表。我们将执行一个非常简单的任务—打印出该字符串中的每个元素。

想到的直观代码可能是这样的:

```
final List<String> strings = Arrays.*asList*("str1", "str2", "str3", "str4", "str5");

int i;
for (i=0; i < strings.size(); i++){
    System.*out*.println(strings.get(i));
}
```