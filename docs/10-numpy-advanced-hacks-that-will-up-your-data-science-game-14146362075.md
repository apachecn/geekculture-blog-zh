# 将提升您的数据科学游戏的 10 个 NumPy 高级技巧

> 原文：<https://medium.com/geekculture/10-numpy-advanced-hacks-that-will-up-your-data-science-game-14146362075?source=collection_archive---------10----------------------->

## 让他们在你的 NumPy 军火库

![](img/2ab8791ab408fae6ad5b498897925c0e.png)

Free for Use Photo from Pexels

# 1.np .非零

非零函数返回数组中非零元素所在的索引列表。

```
**import** numpy **as** nparr = np.random.randint(-4,4, size=10)print(arr)
```