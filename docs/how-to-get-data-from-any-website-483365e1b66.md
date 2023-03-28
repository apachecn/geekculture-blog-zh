# 如何从(任何)网站获取数据

> 原文：<https://medium.com/geekculture/how-to-get-data-from-any-website-483365e1b66?source=collection_archive---------21----------------------->

![](img/e83c6eb188cb7b9dcaa801ca7521d316.png)

众所周知，网络数据几乎触及了我们日常生活的方方面面。当我们工作、购物、旅行和放松时，我们创造、消费并与它互动。毫不奇怪，网络数据对公司的创新和领先于竞争对手产生了影响。但是你如何从网站上获取数据呢？这个叫做“网络抓取”的东西是什么？

# 为什么要从网站中提取数据？

来自其他网站的最新、可信的数据是火箭燃料，可以推动每个组织的成功发展，包括您自己的组织。

你可能想在流行的电子商务网站上比较竞争对手产品的价格。你可以通过在新闻文章和博客中搜索你的品牌的名字来监控顾客的情绪——不管是好是坏。或者你可能正在收集某个特定行业或市场领域的信息，以指导关键的投资决策。

保险承保和信用评分就是 web 数据在金融服务行业中发挥越来越重要作用的一个具体例子。无论是发展中市场还是成熟市场，全球都有数十亿“无形信贷”。虽然这些人没有标准的信用记录，但有大量的“替代数据”来源，可以帮助贷款人评估风险，并有可能将这些人作为客户。这些来源包括借记卡交易和公用事业付款、调查反馈、特定主题的社交媒体帖子以及产品评论。阅读我们的博客，它解释了[公共网络数据如何为金融服务](https://www.zyte.com/blog/insurance-credit-scoring-enhanced-with-alternative-data/)提供商提供精确、有洞察力的备选数据集。

同样在金融领域，对冲基金经理正在转向替代数据——超越公司报告和公告等传统来源的范围——以帮助他们做出投资决策。我们最近在博客上讨论了[网络数据](https://www.zyte.com/blog/alternative-data-for-hedge-funds-portfolio-management/)在这个领域的价值，以及 Zyte 如何帮助提供符合标准的定制数据馈送，以补充传统的研究方法。

简而言之，在了解客户、了解竞争对手的动向，或者根据确凿的事实而非直觉做出任何商业决策时，数据是公司的差异化因素。

网络上有所有这些问题的答案，还有数不清的答案。把它想象成世界上最大、发展最快的研究图书馆。外面有数十亿的网页。然而，与静态库不同的是，当产品价格等细节定期变化时，许多页面会呈现一个移动的目标。无论您是开发人员还是营销经理，获得可靠、及时的网络数据似乎就像在巨大、不断变化的数字大海里捞针。

# 什么是网页抓取？

所以你知道你的业务需要网络数据。接下来会发生什么？通过从其他网站剪切和粘贴您需要的相关信息，没有什么可以阻止您从任何网站手动收集数据。但是这很容易出错，而且对任何负责这项工作的人来说，这都是一项复杂、重复和耗时的工作。当你收集了你需要的所有数据时，不能保证某个特定产品的价格或可用性没有变化。

对于除了最小的项目之外的所有项目，你都需要求助于某种[自动化？]提取液。通常被称为“网络抓取”，[数据提取](https://www.zyte.com/data-extraction/)是抓取相关网络数据的艺术和科学——可能来自几个页面，也可能来自几十万个页面——并以一种组织有序的结构提供给你的企业。

那么数据提取是如何工作的呢？简而言之，它利用计算机来模仿人类在网站上快速、准确、大规模地查找特定信息时的行为。网页主要是为人类的利益而设计的。他们倾向于以我们容易处理、理解和互动的方式呈现信息。例如，如果是一个产品页面，一本书或一双运动鞋的名字很可能会显示在顶部附近，价格也在旁边，可能还有产品的图片。连同隐藏在该网页的 HTML 代码中的大量其他线索，这些视觉指针可以帮助机器以令人印象深刻的准确度找到您想要的数据。

有各种实用的方法来应对提取挑战。最简单的方法是利用现有的各种开源工具。本质上，这些是现成的代码块，它们扫描网页的 HTML 内容，提取出你需要的部分，并将它们归档到某种结构化输出中。走开源路线有一个明显的吸引力，那就是“免费”。但这不是胆小的人的任务，您自己的开发人员将花费大量时间编写脚本和调整现成的代码来满足特定工作的需求。

# 如何从产品页面提取 web 数据的步骤

好了——是时候将这些网络抓取理论付诸实践了。这里有一个工作示例，说明了现实世界提取项目中的三个关键步骤。

# 1.创建提取脚本

为了简单起见，我们将使用[请求](https://docs.python-requests.org/en/master/)和 [beautifulsoup](https://www.crummy.com/software/BeautifulSoup/bs4/doc/) 库来创建我们的脚本。

例如，我将从这个网站提取产品数据:books.toscrape.com

提取脚本将包含两个函数:

1.  寻找产品网址的爬虫
2.  一个真正提取数据的刮刀

发出请求是脚本的一个重要部分:用于查找产品 URL 和获取产品 HTML 文件。首先，让我们从创建一个新类开始，并添加网站的基本 URL:

```
class ProductExtractor(object): 
BASE_URL = 'http://books.toscrape.com'
```

然后，让我们创建一个简单的函数来帮助我们发出请求:

```
import requests 
def make_request(self, url): 
return requests.get(url)
```

函数 *requests.get()* 本身相当简单，但是如果您想用代理扩展您的请求，您只需要修改这部分代码，而不是所有调用 *requests.get()* 的地方。

提取产品 URL

我将只从名为 Travel 的一个类别中提取产品，以获得一些样本数据。在这里，任务基本上是找到这个类别页面上的所有产品 URL，并以某种可迭代的格式返回它们，这样我们就有每个 URL 来请求:

```
from urllib.parse import urljoin 
from bs4 import BeautifulSoup 
def extract_urls(self, start_url): 
response = self.make_request(start_url) 
parser = BeautifulSoup(response.text, 'html.parser') 
product_links = parser.select('article.product_pod > h3 > a') 
for link in product_links: 
relative_url = link.attrs.get('href') 
absolute_url = urljoin(self.BASE_URL, relative_url.replace('../../..', 'catalogue')) 
yield absolute_url
```

这个函数的作用是这样的，一行一行地:

我们发出一个普通的请求来访问类别页面(start_url)

创建一个 BeautifulSoup 对象，它将帮助我们解析类别页面的 HTML

我们使用指定的选择器识别页面上的每个产品 URL 是可用的

迭代提取的链接——此时是元素

通过解析 href 属性，从元素中提取相对 URL

将相对 URL 转换为绝对 URL

返回一个带有绝对 URL 的生成器

# 2.提取产品字段

我们脚本的另一个重要部分是产品提取器函数。

```
def extract_product(self, url): 
response = self.make_request(url) 
parser = BeautifulSoup(response.text, 'html.parser') 
book_title = parser.select_one('div.product_main > h1').text price_text = parser.select_one('p.price_color').text 
stock_info = parser.select_one('p.availability').text.strip() product_data = { 
'title': book_title, 
'price': self.clean_price(price_text), 
'stock': stock_info 
} 
return product_data
```

正如您在上面看到的，对于 price 字段，我需要做一些清理，因为它包含货币和其他字符。幸运的是，有一个开源库可以帮我们解析价格值，它叫做 price_parser(由 Zyte 创建):

```
from price_parser import Price 
def clean_price(self, price_text): 
return Price.fromstring(price_text).amount_float
```

该函数以浮点值的形式返回从文本中提取的产品价格。

# 3.主要功能

最后—这是我们将 extract_urls()和 extract_product()放在一起的主函数。

```
def main(): 
extractor = ProductExtractor() 
product_urls = extractor.extract_urls('http://books.toscrape.com/catalogue/category/books/travel_2/index.html') extracted_data = [] 
for url in product_urls: 
product_data = extractor.extract_product(url) extracted_data.append(product_data) extractor.export_json(extracted_data, 'data.json') 
And the export_json() function: 
import json 
def export_json(self, data, file_name): 
with open(file_name, 'w') as f: 
json.dump(data, f) 
The end result is a clean json data file, something like this: 
[ 
{ 
"title": "It's Only the Himalayas", 
"price": 45.17, 
"stock": "In stock (19 available)" 
}, 
{ 
"title": "Full Moon over Noah\u00e2\u0080\u0099s Ark: An Odyssey to Mount Ararat and Beyond", 
"price": 49.43, 
"stock": "In stock (15 available)" 
}, 
{ 
"title": "See America: A Celebration of Our National Parks & Treasured Sites", 
"price": 48.87, 
"stock": "In stock (14 available)" 
}, 
{ 
"title": "Vagabonding: An Uncommon Guide to the Art of Long-Term World Travel", 
"price": 36.94, 
"stock": "In stock (8 available)" 
}, 
{ 
"title": "Under the Tuscan Sun", 
"price": 37.33, 
"stock": "In stock (7 available)" 
}, 
{ 
"title": "A Summer In Europe", 
"price": 44.34, 
"stock": "In stock (7 available)" 
}, 
{ 
"title": "The Great Railway Bazaar", 
"price": 30.54, 
"stock": "In stock (6 available)" 
}, 
{ 
"title": "A Year in Provence (Provence #1)", 
"price": 56.88, 
"stock": "In stock (6 available)" 
}, 
{ 
"title": "The Road to Little Dribbling: Adventures of an American in Britain (Notes From a Small Island #2)", 
"price": 23.21, 
"stock": "In stock (3 available)" 
}, 
{ 
"title": "Neither Here nor There: Travels in Europe", 
"price": 38.95, 
"stock": "In stock (3 available)" 
}, 
{ 
"title": "1,000 Places to See Before You Die", 
"price": 26.08, 
"stock": "In stock (1 available)" 
} 
]
```

# 为什么您需要使用智能代理进行 web 抓取？

在任何网络抓取项目的过程中，都有很多陷阱需要解决。当你试图提取大规模数据时，最大的挑战之一就来了。

在 Zyte，我们经常与那些每天成功从一百或一千个网页中提取数据的客户交谈。他们肯定会问，每天从一百万页中获取数据一定很容易吧？

许多网站使用“反机器人”技术来阻止自动抓取。有很多方法可以解决这个问题，最有效的方法是使用智能旋转代理。这是一种有效地哄骗目标网站的技术，让它认为它正在被一个人无害地访问，而不是一个提取脚本。

这里有一个例子，展示了 Zyte 的[智能代理管理器](https://www.zyte.com/smart-proxy-manager/)如何集成到数据提取脚本中，以增加你被禁止的机会。

还记得我们在开始时创建了一个 make_request()函数，以便它处理脚本中的所有请求吗？现在，如果我们想使用智能代理管理器，我们只需要在这个功能上做一个小的改变。其他一切都会很好。要集成智能代理管理器，请更改此功能:

```
def make_request(self, url): return requests.get(url)
```

对此:

```
def make_request(self, url): 
zyte_apikey = 'apikey' 
proxy_url = 'proxy.zyte.com:8011' 
return requests.get( 
url, 
proxies={ 
"http": "http://{}:@{}".format(zyte_apikey, proxy_url), 
}, 
)
```

在这段代码中，我们添加智能代理管理器端点作为代理，并使用 Zyte API 密钥进行身份验证。

如果您想了解更多关于[智能代理管理器以及它如何帮助您扩展](https://www.zyte.com/blog/alternative-data-for-hedge-funds-portfolio-management/)，请查看我们的网络研讨会。

## Zyte 如何帮助你提取你想要的 web 数据？

在 Zyte，我们花了十年的大部分时间专注于提取公司需要的所有重要的网络数据。我们的国际开发人员和数据科学家团队包括分析、人工智能和机器学习领域的一些最大的大脑。在这一过程中，我们开发了一些强大的工具，其中一些受到国际专利保护，以帮助我们的客户快速、可靠且经济高效地实现数据提取目标。

【https://www.zyte.com】最初发表于[](https://www.zyte.com/blog/how-to-extract-data-from-a-website/)**。**