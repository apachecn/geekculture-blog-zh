# 有效的 Java！明智地使用重载

> 原文：<https://medium.com/geekculture/effective-java-use-overloading-judiciously-b326734e64a?source=collection_archive---------44----------------------->

为了介绍我们今天的章节主题，让我们看一些试图确定参数类型的代码:

```
public class CollectionClassifier {
  public static String classify(Set<?> ser) {
    return "Set";
  } public static String classify(List<?> list) {
    return "List";
  } public static String classify(Collection<?> collection) {
    return "Unknown Collection";
  } public static void main(String[] args) {
    Collection<?>[] collections = {
      new…
```