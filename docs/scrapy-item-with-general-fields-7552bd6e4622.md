# 带有常规字段的零碎项目

> 原文：<https://medium.com/geekculture/scrapy-item-with-general-fields-7552bd6e4622?source=collection_archive---------0----------------------->

## 常规/未知/动态字段

源代码你可以在这里找到。它作为[报废项目](https://github.com/alex-ber/scrapy-item)的一部分提供。

您可以从 PyPi 安装 scrapy-item:

```
python3 -m pip install -U scrapy-item
```

你也应该安装 Scrapy。

有时，您正在抓取一个网页或调用接收来自一些具有动态结构的 Web 服务的响应——可能它只是一个表结构，您想要形成项目和字段属性，或者您可能接收到不同的结果结构，这取决于 REST API 调用的一些状态。或者你有很多属性，它们可能会根据你传递给请求的参数而改变。

您可以直接使用我的 GeneralItem，或者(更好地)定义从它继承的项目，如下所示:

使用示例:

另一个例子:

您可能还想查看一下 [itemadapter](https://github.com/scrapy/itemadapter) 项目:

> `ItemAdapter`类是数据容器对象的包装器，提供一个通用接口以统一的方式处理不同类型的对象，而不管它们的底层实现。
> 
> 当前支持的类型有:
> 
> `[dict](https://docs.python.org/3/library/stdtypes.html#dict)`
> 
> `[scrapy.item.Item](https://docs.scrapy.org/en/latest/topics/items.html#scrapy.item.Item)`
> 
> `[dataclass](https://docs.python.org/3/library/dataclasses.html)`基础类
> 
> `[attrs](https://www.attrs.org)`基础类
> …

```
>>> from scrapy.item import Item, Field
>>> from itemadapter import ItemAdapter
>>> class InventoryItem(Item):
...     name = Field()
...     price = Field()
...
>>> item = InventoryItem(name="foo", price=10)
>>> adapter = ItemAdapter(item)
>>> adapter.item is item
True
>>> adapter["name"]
'foo'
>>> adapter["name"] = "bar"
>>> adapter["price"] = 5
>>> item
{'name': 'bar', 'price': 5}
```

[https://github.com/scrapy/itemadapter](https://github.com/scrapy/itemadapter)

这个项目的首次发布是在 2020 年 4 月 25 日。这是 Scrapy 的子项目。

**参见:**

[Scrapy 直播流数据](/@alex_ber/scrapy-live-streaming-data-cdf97434dc15)

[作业运行之间的杂乱状态](/@alex_ber/scrapy-state-between-job-runs-b880c7b34a9d)

[带有通用字段的刺儿头项目](/@alex_ber/scrapy-item-with-general-fields-7552bd6e4622)