# Python 深层拷贝与浅层拷贝

> 原文：<https://medium.com/geekculture/python-deep-copy-vs-shallow-copy-3580e80f9d83?source=collection_archive---------3----------------------->

## 你能解释一下浅层拷贝和深层拷贝的区别吗？

![](img/3a1c8f4a42c19fbe707bf5a5512b6f27.png)

让我们从一个简单的问题开始，看看下面的代码片段:

```
list_1 = [1, 2, 3]
list_2 = list(list_1)
```

可以回答一下，`list_2`是`list_1`的浅拷贝还是深拷贝吗？Python 中的“深度复制/浅层复制”是什么意思？为什么他们…