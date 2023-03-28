# Java 5+ (2005) / C++11/C11 内存模型似乎是由同一批人发明的

> 原文：<https://medium.com/geekculture/java-5-2005-c-11-c11-memory-model-appears-that-have-been-invented-by-the-same-people-efa2cb6576e2?source=collection_archive---------30----------------------->

C++及 2012 年以后:赫伯·萨特——原子武器

[下载幻灯片。](https://1drv.ms/b/s!Aq0V7yDPsIZOgcI0y2P8R-VifbnTtw)

这可能会让你吃惊，但实际上直到 Java 5.0，许多单例实现都有一个 bug。它是如此广泛的分布器，以至于 Java 内存模型被修复以使有缺陷的代码工作。在 C++中也有同样的问题，在 C ++ 11 中已经解决了。

C/C++中的`Atomic` 与 Java 5+ `volatile`(或`AtomicInteger`、`AtomicLong` 或另一个*原子** )

`Atomics` 相同，允许写`CAS`(或交换)，进而允许写`e lock-free algorithm`。

另见:
[http://www . e-reading . club/book reader . PHP/134637/Herlihy，_ Shavit _-_ The _ art _ of _ multiprocessor _ programming . pdf](http://www.e-reading.club/bookreader.php/134637/Herlihy,_Shavit_-_The_art_of_multiprocessor_programming.pdf)

引用:

> Ab *stract:
> 
> 这一届就一个字:深。
> 令人痛心的是，这一点非常清楚，因为一些硬件鼓励你拿出大枪来实现最高性能，而 C++程序员太沉迷于完整的性能，以至于他们会伸手去拿闪着警告灯的红色大杆。既然我们不能阻止人们拉红色的大杠杆，我们最好记录下杠杆实际上做了什么，这样人们就不会急停，除非他们真的，真的，真的想这么做。
> 
> 涵盖的主题:
> 
> *事实:C++11 内存模型以及它要求您做什么来确保您的代码是正确的并保持正确。我们将包括几个常见问题的明确答案:“编译器和硬件如何合作来记住如何遵守这些规则？”，“什么是竞态条件？”，以及永恒的令人拍手称快的问题“竞争条件如何像调试器？”
> 
> *工具:互斥、原子和栅栏/障碍之间的深层相互关系和基本权衡。我将尝试说服您为什么独立的内存屏障不好，以及为什么屏障应该总是与特定的加载或存储相关联。
> 
> *不可说的:我将勉强和不情愿地谈论我说过我永远不会教程序员现在永远不需要教的东西:宽松的原子。不要用它们！如果可以避免的话。但是这里有你需要知道的，即使你不需要知道也很好。
> 我们将介绍最新的 CPU 和 GPU 硬件内存模型是如何快速发展的，以及这如何直接影响 C++程序员。*

[https://channel 9 . msdn . com/Shows/Going+Deep/Cpp-and-Beyond-2012-Herb-Sutter-atomic-Weapons-1-of-2](https://channel9.msdn.com/Shows/Going+Deep/Cpp-and-Beyond-2012-Herb-Sutter-atomic-Weapons-1-of-2)

[https://channel 9 . msdn . com/Shows/Going+Deep/Cpp-and-Beyond-2012-Herb-Sutter-atomic-Weapons-2-of-2](https://channel9.msdn.com/Shows/Going+Deep/Cpp-and-Beyond-2012-Herb-Sutter-atomic-Weapons-2-of-2)

*原载于我的博客*[*https://www . toalexsmail . com/2017/11/c-and-beyond-2012-herb-sutter-atomic . html*](https://www.toalexsmail.com/2017/11/c-and-beyond-2012-herb-sutter-atomic.html)