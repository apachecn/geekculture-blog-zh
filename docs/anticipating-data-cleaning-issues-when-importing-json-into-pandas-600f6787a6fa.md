# 预测将 JSON 导入 Pandas 时的数据清理问题

> 原文：<https://medium.com/geekculture/anticipating-data-cleaning-issues-when-importing-json-into-pandas-600f6787a6fa?source=collection_archive---------25----------------------->

**JavaScript 对象符号** ( **JSON** )已经成为将数据从一台机器、进程或节点传输到另一台机器、进程或节点的非常有用的标准。通常，客户端向服务器发送数据请求，服务器查询本地存储中的数据，然后将数据从 SQL Server 表转换成客户端可以使用的 JSON。有时，第一个服务器(比如说 web 服务器)将请求转发给数据库服务器会使事情变得更加复杂。JSON 通过以下方式促进了这一点:

*   可供人类阅读
*   可由大多数客户端设备使用
*   在结构上不受限制

JSON 非常灵活，这意味着它可以适应任何东西。JSON 文件中的结构甚至可以改变，因此不同的键可能出现在不同的位置。例如，文件可能以一些解释键开始，这些解释键与其余的*数据*键具有非常不同的结构。或者某些键在某些情况下可能存在，但在其他情况下不存在。我们回顾了一些处理这种混乱的方法(嗯，我指的是灵活性)。

# 在我们开始之前…

这篇文章摘自《Python 数据清理》一书中的一章，这本书是我写的，由 Packt 出版。可以在这里购买:[https://www . Amazon . com/Python-Data-Cleaning-Cookbook-techniques-ebook/DP/b 08 MB 9d 3hz/ref = Sr _ 1 _ 3？dchild=1 &关键字= python+data+cleaning+cookbook&qid = 1619866892&s = digital-text&Sr = 1-3](https://www.amazon.com/Python-Data-Cleaning-Cookbook-techniques-ebook/dp/B08MB9D3HZ/ref=sr_1_3?dchild=1&keywords=python+data+cleaning+cookbook&qid=1619866892&s=digital-text&sr=1-3)。

代码可以从 GitHub 的[https://GitHub . com/packt publishing/Python-Data-Cleaning-Cookbook](https://github.com/PacktPublishing/Python-Data-Cleaning-Cookbook)下载。

在本帖中，我们将使用关于政治候选人的新闻报道数据。这些数据在[dataverse.harvard.edu/dataset.xhtml?persistentId=doi](http://dataverse.harvard.edu/dataset.xhtml?persistentId=doi):10.7910/DVN/0 兹洛克可供公众使用。我将那里的 JSON 文件合并成一个文件，并从合并的数据中随机选择了 60，000 个新闻故事。这个示例(`allcandidatenewssample.json`)可以在本书的 GitHub 资源库中找到。

# 怎么做…

在做了一些数据检查和清理之后，我们将把一个 JSON 文件导入 pandas:

1.  导入`json,` `pprint,`和`Counter`。

改进了加载 JSON 数据时返回的列表和字典的显示。在这篇文章的后面，我们会看到当我们需要对加载的数据做一些初步的统计时,`collections`模块非常有用。

```
>>> import pandas as pd>>> import numpy as np >>> import json >>> import pprint >>> from collections import Counter 
```

2.加载 JSON 数据并寻找潜在的问题。

使用`json load`方法返回关于政治候选人的新闻报道的数据。`load`返回词典列表。使用`len`获得列表的大小，这是本例中新闻故事的总数。(每个列表项都是一个字典，包含标题、源等的关键字以及它们各自的值。)使用`pprint`显示前两个字典。从源键获取第一个列表项的值:

```
>>> with open('data/allcandidatenewssample.json') as f:
 ...   candidatenews = json.load(f)
 ... 

>>> len(candidatenews) 
60000 >>> pprint.pprint(candidatenews[0:2]) 
[{'date': '2019-12-25 10:00:00',
  'domain': 'www.nbcnews.com',
  'panel_position': 1,  
  'query': 'Michael Bloomberg', 
  'source': 'NBC News',  
  'story_position': 6,  
  'time': '18 hours ago', 
  'title': 'Bloomberg cuts ties with company using prison inmates to         make campaign calls',  
  'url': 'https://www.nbcnews.com/politics/2020-election/bloomberg-cuts-ties-company-using-prison-inmates-make-campaign-calls-n1106971'}, 
 {'date': '2019-11-09 08:00:00',  
  'domain': 'www.townandcountrymag.com',  
  'panel_position': 1,  
  'query': 'Amy Klobuchar',  
  'source': 'Town & Country Magazine', 
  'story_position': 3,  
  'time': '18 hours ago',  
  'title': "Democratic Candidates React to Michael Bloomberg's Potential Run",  
  'url': https://www.townandcountrymag.com/society/politics/a29739854/michael-bloomberg-democratic-candidates-campaign-reactions/'}] >>> pprint.pprint(candidatenews[0]['source']) 'NBC News'
```

3.检查字典结构的差异。

使用`Counter`检查列表中任何少于或多于九个正常键的词典。在删除它们之前，查看几个几乎没有数据的字典(只有两个关键字的字典)。确认剩余的词典列表具有预期的长度-*60000-2382 = 57618*:

```
>>> Counter([len(item) for item in candidatenews])
Counter({9: 57202, 2: 2382, 10: 416})>>> pprint.pprint(next(item for item in candidatenews if len(item)<9))
{'date': '2019-09-11 18:00:00', 'reason': 'Not collected'}>>> pprint.pprint(next(item for item in candidatenews if len(item)>9))
{'category': 'Satire',
 'date': '2019-08-21 04:00:00',
 'domain': 'politics.theonion.com',
 'panel_position': 1,
 'query': 'John Hickenlooper',
 'source': 'Politics | The Onion',
 'story_position': 8,
 'time': '4 days ago',
 'title': ''And Then There Were 23,' Says Wayne Messam Crossing Out '
          'Hickenlooper Photo \n'
          'In Elaborate Grid Of Rivals',
 'url': '[https://politics.theonion.com/and-then-there-were-23-says-wayne-messam-crossing-ou-1837311060'](https://politics.theonion.com/and-then-there-were-23-says-wayne-messam-crossing-ou-1837311060')}
>>> pprint.pprint([item for item in candidatenews if len(item)==2][0:10])
[{'date': '2019-09-11 18:00:00', 'reason': 'Not collected'},
 {'date': '2019-07-24 00:00:00', 'reason': 'No Top stories'},
... 
 {'date': '2019-01-03 00:00:00', 'reason': 'No Top stories'}]>>> candidatenews = [item for item in candidatenews if len(item)>2]>>> len(candidatenews)
57618
```

4.从 JSON 数据生成计数。

为政治(一个报道政治新闻的网站)买几本字典，展示几本字典:

```
>>> politico = [item for item in candidatenews if item["source"] == "Politico"]>>> len(politico)
2732>>> pprint.pprint(politico[0:2])
[{'date': '2019-05-18 18:00:00',
  'domain': '[www.politico.com'](http://www.politico.com'),
  'panel_position': 1,
  'query': 'Marianne Williamson',
  'source': 'Politico',
  'story_position': 7,
  'time': '1 week ago',
  'title': 'Marianne Williamson reaches donor threshold for Dem debates',
  'url': '[https://www.politico.com/story/2019/05/09/marianne-williamson-2020-election-1315133'](https://www.politico.com/story/2019/05/09/marianne-williamson-2020-election-1315133')},
 {'date': '2018-12-27 06:00:00',
  'domain': '[www.politico.com'](http://www.politico.com'),
  'panel_position': 1,
  'query': 'Julian Castro',
  'source': 'Politico',
  'story_position': 1,
  'time': '1 hour ago',
  'title': "O'Rourke and Castro on collision course in Texas",
  'url': '[https://www.politico.com/story/2018/12/27/orourke-julian-castro-collision-texas-election-1073720'](https://www.politico.com/story/2018/12/27/orourke-julian-castro-collision-texas-election-1073720')}]
```

5.获取`source`数据并确认其具有预期长度。

显示新`sources`列表中的前几个项目。按来源生成新闻故事的计数，并显示 10 个最受欢迎的来源。请注意，来自 *The Hill* 的故事可以将`TheHill`(不带空格)或`The Hill`作为`source`的值:

```
>>> sources = [item.get('source') for item in candidatenews]>>> type(sources)
<class 'list'>>>> len(sources)
57618>>> sources[0:5]
['NBC News', 'Town & Country Magazine', 'TheHill', 'CNBC.com', 'Fox News']>>> pprint.pprint(Counter(sources).most_common(10))
[('Fox News', 3530),
 ('CNN.com', 2750),
 ('Politico', 2732),
 ('TheHill', 2383),
 ('The New York Times', 1804),
 ('Washington Post', 1770),
 ('Washington Examiner', 1655),
 ('The Hill', 1342),
 ('New York Post', 1275),
 ('Vox', 941)]
```

6.修复字典中值的任何错误。

固定`The Hill`的`source`值。请注意`The Hill`现在是新闻故事最常见的来源:

```
>>> for newsdict in candidatenews:
...     newsdict.update((k, "The Hill") for k, v in newsdict.items()
...      if k == "source" and v == "TheHill")
... >>> sources = [item.get('source') for item in candidatenews]>>> pprint.pprint(Counter(sources).most_common(10))
[('The Hill', 3725),
 ('Fox News', 3530),
 ('CNN.com', 2750),
 ('Politico', 2732),
 ('The New York Times', 1804),
 ('Washington Post', 1770),
 ('Washington Examiner', 1655),
 ('New York Post', 1275),
 ('Vox', 941),
 ('Breitbart', 799)]
```

7.创建一个熊猫数据框架。

将 JSON 数据传递给 pandas `DataFrame`方法。将`date`列转换为`datetime`数据类型:

```
>>> candidatenewsdf = pd.DataFrame(candidatenews)>>> candidatenewsdf.dtypes
title             object
url               object
source            object
time              object
date              object
query             object
story_position     int64
panel_position    object
domain            object
category          object
dtype: object
```

8.确认我们正在获取`source`的预期值。

另外，重命名`date`列:

```
>>> candidatenewsdf.rename(columns={'date':'storydate'}, inplace=True)>>> candidatenewsdf.storydate = candidatenewsdf.storydate.astype('datetime64[ns]')>>> candidatenewsdf.shape
(57618, 10)>>> candidatenewsdf.source.value_counts(sort=True).head(10)
The Hill               3725
Fox News               3530
CNN.com                2750
Politico               2732
The New York Times     1804
Washington Post        1770
Washington Examiner    1655
New York Post          1275
Vox                     941
Breitbart               799
Name: source, dtype: int64
```

我们现在有一个熊猫数据框架，其中只有新闻故事，有有意义的数据，并且`source`的值是固定的。

# 它是如何工作的…

方法返回一个字典列表。这使得在处理这些数据时可以使用许多熟悉的工具:列表方法、切片、列表理解、字典更新等等。有时候，也许你只需要填充一个列表或者计算一个给定类别中的个体数量，就没有必要使用熊猫了。

在*步骤 2* 到 *6* 中，我们使用列表方法做许多我们通常对熊猫做的相同检查。在*步骤 3* 中，我们使用`Counter`和列表理解(`Counter([len(item) for item in candidatenews])`)来获得每个字典中的键的数量。这告诉我们有 2382 本字典只有 2 个键，416 本有 10 个键。我们使用`next`来寻找少于 9 个键或多于 9 个键的字典的例子，以了解这些条目的结构。我们使用切片来显示 10 个带有 2 个键的字典，以查看这些字典中是否有数据。然后我们只选择那些超过 2 个关键字的字典。

在*步骤 4* 中，我们创建了字典列表的一个子集，其中`source`等于`Politico`，并查看几个条目。然后，我们创建一个只包含源数据的列表，并在*步骤 5* 中使用`Counter`列出 10 个最常见的源。

*步骤 6* 演示了如何在字典列表中有条件地替换键值。在这种情况下，每当`key (k)`为`source`并且`value (v)`为`TheHill`时，我们就将键值更新为`The Hill`。`for k, v in newsdict.items()`段是这条线的无名英雄。它遍历`candidatenews`中所有字典的所有键/值对。

通过将字典列表传递给 pandas `DataFrame`方法，很容易创建 pandas 数据框架。我们在*步骤 7* 中这样做。主要的复杂之处在于，我们需要将日期列从字符串转换成日期，因为日期在 JSON 中只是字符串。

# 还有更多…

在*步骤 5* 和*步骤 6* 中，我们使用`item.get('source')`代替`item['source']`。当字典中可能缺少键时，这很方便。当键丢失时，`get`返回`None`，但是我们可以使用可选的第二个参数来指定要返回的值。

我在*步骤 8* 中将`date`列重命名为`storydate`。这不是必需的，但却是一个好主意。`date`不仅没有告诉您日期实际代表什么，它还是一个如此通用的列名，以至于在某些时候一定会引起问题。

新闻故事数据非常适合表格结构。将每个列表项表示为一行，将键/值对表示为该行的列和列值是有意义的。没有明显的复杂性，比如键值本身就是字典列表。想象一下，每个故事有一个`authors`键，每个作者有一个列表项作为键值，这个列表项是关于作者的信息的字典。在 Python 中处理 JSON 数据时，这一点也不奇怪。我的书《Python 数据清理指南》也研究了 JSON 结构更复杂的情况。