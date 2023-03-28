# 处理熊猫数据框架中的空值

> 原文：<https://medium.com/geekculture/dealing-with-null-values-in-pandas-dataframe-1a67854fe834?source=collection_archive---------3----------------------->

价值观缺失问题在现实世界中非常普遍。例如，假设您正试图从一家公司收集信息。有一个用于*公司地址*的字段。许多人希望保护自己的隐私，将此字段留空。如果数据是由 pandas 加载的，这些空字段将被列为**缺失值**。`NaN`是熊猫中缺省的缺失值。在这篇文章中，让我们看看如何处理它们。

![](img/5f13bef2b80afd9a69f1ed3435c85fe9.png)

Photo by [Ben Hershey](https://unsplash.com/@benhershey?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 对`NaN`数据的常见操作

*   当对包含缺失值的序列执行`mean` / `sum` / `std` / `median`时，这些值将被视为零。
*   执行`add` / `div` / `sub`时，结果为`NaN`。
*   如果所有数据都是`NaN`，则`sum()`结果为零。

`pd.Series([np.nan]).sum() #result is zer`

```
import pandas as pd
s = pd.Series([1, 2, None, 4])
print(s)
s1 = pd.Series([2, 3, 5, 6])# The third value is NA
print(s + s1)# sum a series only contains NaN
s2 = pd.Series([None, None])
print(s2.sum())
```

`line 3`显示包含一个`NaN`元素的 Series 对象的总和。`line 7`显示添加两个系列对象，其中一个包含一个`NaN`元素。`line 11`显示只包含`NaN`的 a 系列求和的结果。

## NaN 上的 Groupby 操作

如上所述，`NaN`在大多数操作中会被视为零。然而，在`groupby`中，`NaN`被自动排除。

```
import pandas as pd
data = {“key”: [“K0”, “K0”, “K0”, None, None], “val”: [1, 2, 3, 4, 5]}group = pd.DataFrame(data).groupby(“key”).mean()
print(group)
```

# NaN 值的函数运算

## `isnull`功能

为了检查数据是否为 **NA** ，`isnull()`返回一个大小相同的布尔值数据帧。当值为`NaN`时，对应位置为真，否则为假

```
import pandas as pd
s = pd.Series([1, 2, None, 4])
print(“The original series object.”)
print(s)
print(“The result after calling isnull”)
print(s.isnull())
```

如`isnull()`的输出所示，它是一个与原始对象长度相同的布尔值序列对象。

## 菲尔纳函数

pandas 提供了一个非常有用的函数来填充缺失值，`fillna()`

正常情况下，`fillna()`只要传递一个静态描述值就足够解决问题了。

```
improt pandas as pd
s = pd.Series([1, 2, None, 4])
s.fillna(6)
```

然而，当您处理时间序列数据时，用最后一个非缺失值填充缺失值是非常常见的。

*   `fillna(method='ffill')`从下一个值中填充缺少的值。
*   `fillna(method='bfill')`从最后一个值或最后一个非缺失值开始填充缺失值。
*   `ffill`相当于`fillna(method='ffill')`。
*   `bfill`相当于`fillna(method='bfill')`。

```
import pandas as pd
s = pd.Series([1,2,None,None,5,6])
print(“The original value”)
print(s.values)
print(“Fill the value by forward method”)
print(“The None value is filled by the last seen value \”2\””)
s1 = s.fillna(method=”ffill”)
print(s1.values)
```

在这个例子中，我们用最后看到的值`2`填充这些`NaN`值。

## 丢弃`NaN`数据

NaN 数据上最常用的函数，为了从数据帧中删除一个`NaN`值，我们使用了`dropna()`函数。该函数删除具有`NaN`值的数据行/列。

*   `dropna()` -删除至少有一个`NaN`值的行。
*   `dropna(how = 'all')` -删除所有值都为`NaN`的行。
*   `dropna(axis = 1)` -删除至少有一个`NaN`值的列。

```
import pandas as pdd = {"a": [1, 2, 3, None],"b": [1, None, 3, None],"c": [1, 2, 3, None],"d": [1, 2, 3, None]} df = pd.DataFrame(d)
print("The original DataFrame")
print(df)print("-----------------------------")df1 = df.dropna()
print("Rows with index 1 and 3 are dropped.")
print(df1)print("-----------------------------")print("Rows with index 3 are dropped, whose values are all NA")
df2 = df.dropna(how='all')
print(df2)print("-----------------------------")df['e'] = df['a'].fillna(1) + 1
print("Column a b c d are dropped")
df3 = df.dropna(axis=1)
print(df3)
```

`line 13`显示了如何删除至少包含一个`NaN`元素的行。

`line 19`展示了如何删除所有元素都是`NaN`的行。

`line 25`显示了如何删除至少包含一个`NaN`元素的列。

空值在现实世界中很常见。处理空值将有助于我们在进行繁重操作时优化性能，并使数据帧更加健壮。下次见，再见！