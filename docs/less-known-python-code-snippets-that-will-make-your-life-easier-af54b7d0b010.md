# 鲜为人知的 Python 代码片段将使您的生活更加轻松！

> 原文：<https://medium.com/geekculture/less-known-python-code-snippets-that-will-make-your-life-easier-af54b7d0b010?source=collection_archive---------10----------------------->

## 有时候，你只需要几行代码就可以完成你想要的

参考一个类似的[帖子](/geekculture/16-python-and-pandas-hacks-that-will-save-you-time-in-your-project-292da3df0931?source=your_stories_page-------------------------------------)，同样会对你有帮助！

我们开始吧！

# 1.获取对象的大小

```
**import** syslong_range = range(0,1000000)
print(sys.getsizeof(long_range))
>> 48
```

# 2.为安全起见，将字符串编码成散列…