# 如何准确快速的确定一个网页使用的总内存？

> 原文：<https://medium.com/geekculture/how-to-determine-exactly-and-quickly-the-total-memory-used-by-a-web-page-d54cc3d90b46?source=collection_archive---------10----------------------->

## 不是堆大小，而是单个网页使用的总内存。

为了演示如何准确快速地测量一个 web 页面使用的总内存，我将使用一个脚本，首先逐渐填充内存，然后逐渐释放内存。

```
let a, b, c;printMemory('baseline')
    .then(() => {
        a = makeString(10);
        return printMemory('a = makeString(10)');
    })
    .then(() => {
        b =…
```