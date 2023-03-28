# 16 条 Python 和 Pandas Hacks 将为您的项目节省时间

> 原文：<https://medium.com/geekculture/16-python-and-pandas-hacks-that-will-save-you-time-in-your-project-292da3df0931?source=collection_archive---------0----------------------->

## 让您的数据科学工作流程更加高效

![](img/e5b636eff3046de2ce5ec4811505795e.png)

Free for Use photo from [Pexels](https://www.pexels.com/ko-kr/photo/8867217/)

## **1。检查对象的内存使用情况**

```
import syslst = range(0, 500)print(sys.getsizeof(lst))
```

## **2。将字符串转换为字节**