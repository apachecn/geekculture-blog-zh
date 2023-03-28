# 机器学习中“random.seed(42)”背后的故事

> 原文：<https://medium.com/geekculture/the-story-behind-random-seed-42-in-machine-learning-b838c4ac290a?source=collection_archive---------1----------------------->

## PYTHON CHARMERS 俱乐部

## “生命、宇宙、万物”这个大问题的答案！

![](img/2a898801c5b04b65da1a5716343c266f.png)

📷 — [Ihor Malytskyi](https://unsplash.com/@ihor_malytskyi?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

你一定曾经在数据科学/机器学习中遇到过 *random.seed(42)* 。那么这意味着什么呢？

**首先，我们来复习一些基础知识:**

# 什么是 random()？

*random()* 是 Python 中用来生成**伪随机数**的函数。

计算机算法只能产生看似随机的(或伪随机数)，尽管可以结合一些自然发生的现象来人工创造随机性。

![](img/0ae537f1c5495880ad0fbd1e8e740364.png)

对于 *random()* 函数，Python 使用[**Mersenne Twister**](https://documentation.help/Python-2.5/node112.html)作为核心生成器。它产生 53 位精度浮点，周期为 2 * * 19937–1。Mersenne Twister 也是现存测试最广泛的随机数生成器之一。由于是完全确定性的，它并不适用于所有目的，并且完全不适用于加密目的。

# 什么是 random.seed()？

*random.seed()* 函数用给定值初始化随机数发生器。

*random.seed(a=None，version=2)* 函数采用以下两个参数:

*   如果省略 *a* 或 *None* ，则使用当前系统时间。如果操作系统提供了随机源，则使用它们来代替系统时间。而如果 *a* 是 *int* 则直接使用。
*   使用 *version=2* (从 Python 3.2 开始成为默认设置)，一个 *str* 、 *bytes* 或 *bytearray* 对象被转换成一个 *int* ，并且它的所有位都被使用。在版本 1 中，使用的是 *a* 的 *hash()* 。

种子被赋予整数值，以确保伪随机生成的结果是可再现的。通过重用种子值，只要没有运行多个线程，就应该可以在不同的运行之间重复相同的序列。**可再现性**是一个非常重要的概念，它确保任何人重新运行代码都会得到完全相同的输出。

**输入**

```
import random
for i in range(5):
    random.seed(1)
    print(random.randint(1, 1000))
```

**输出**

```
138
138
138
138
138
```

# random.seed(42)的意义是什么？

这是一个流行文化的参考！

在道格拉斯·亚当斯 1979 年的热门科幻小说 ***《银河系漫游指南》****中，在书的结尾，超级计算机[深思](https://hitchhikers.fandom.com/wiki/Deep_Thought)揭示了“生命、宇宙和万物”这个重大问题的答案是 42。*

> *【750 万年后……福和伦克威尔早已不在人世，但他们的后代继续着他们开创的事业]*
> 
> *“好吧，”深思说。“伟大问题的答案…”
> “是的..!"“生命、宇宙和一切……”深思说。
> “是啊……”
> “是……”深以为然的说道，顿了顿。
> “是的…！”
> “是……”
> “是……！！！…?"
> “四十二，”深思说，带着无限的威严和平静。"*
> 
> ***――道格拉斯·亚当斯，银河系漫游指南***

*![](img/d30960c794873a6345a445beea9c0df3.png)*

*📷 — screen grab from the [movie](https://www.imdb.com/title/tt0371724/)*

*亚当斯选择 42 号已经成为了极客文化的一部分。人们从中发现幽默，有些人出于对经典科幻文学的崇敬，在不同的地方使用 42。*

*球迷们有许多理论试图解释为什么选择 42 号。*

*   *一些人认为选择它是因为 42 在二进制编码中是 101010，另一些人指出光在水面上折射 42 度形成彩虹。*
*   *其他人评论说，光穿过一个质子的直径需要 10⁻⁴秒。*
*   *还有人说 42 是亚当斯对不知疲倦的平装本书的致敬，是普通平装本的平均页数。*
*   *另一个普遍的理论是，42 指的是板球比赛中的法律数量，这是书中反复出现的主题。*

*然而，道格拉斯·亚当斯本人在[这则消息](https://groups.google.com/g/alt.fan.douglas-adams/c/595nPukE-Jo/m/koaAJ3tPBtEJ)中透露了他选择 42 的原因。“这是个笑话。它必须是一个数字，一个普通的小数字，我选择了它。我坐在书桌前，凝视着花园，心想‘42 号就够了！’"*

*如果你还没有阅读 ***银河系漫游指南*** 的话，继续练习 Python 编程吧！🐍*

*[](https://www.goodreads.com/book/show/386162.The_Hitchhiker_s_Guide_to_the_Galaxy) [## 银河系漫游指南(银河系漫游指南，#1)

### 银河系漫游指南。阅读来自世界上最大的读者社区的 33，357 条评论。一个…

www.goodreads.com](https://www.goodreads.com/book/show/386162.The_Hitchhiker_s_Guide_to_the_Galaxy) 

我希望这篇文章对你有用。可以在 LinkedIn *上和我* [*联系，也可以在这里*](http://linkedin.com/in/nazianafis) *关注我的著作* [。](https://nazianafis.medium.com/)

*下次见！*(∫･‿･)ﾉ゛*