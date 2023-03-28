# 理解 Python 的“屈服”，一个中断，一个陷阱，一把剪刀

> 原文：<https://medium.com/geekculture/understand-python-yield-an-interrupter-a-trap-a-scissor-27c2beeed73?source=collection_archive---------24----------------------->

有人说“yield”类似于“return”的用法，答案可能是肯定的，也可能是否定的。但有一点是肯定的:`yield`不是`return`。

# Python 中 yield 是什么？

关键字“yield”像剪刀一样把程序剪成两部分。

关键字“yield”工作起来就像 OS 中的一个中断，yield 会把程序的控制权交还给调用者。

关键字“yield”不仅将控制权交还给调用者，还可以…