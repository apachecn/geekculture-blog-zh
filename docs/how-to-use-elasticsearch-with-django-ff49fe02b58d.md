# 如何使用 Django 的 ElasticSearch

> 原文：<https://medium.com/geekculture/how-to-use-elasticsearch-with-django-ff49fe02b58d?source=collection_archive---------0----------------------->

![](img/975cd8ad1a62243a1b0c1ff8d4998d28.png)

ElasticSearch with Django

# 什么是 Elasticsearch？

Elasticsearch 是一个基于 Lucene 库的搜索引擎。它提供了一个分布式的、支持多租户的全文搜索引擎，带有 HTTP web 接口和无模式的 JSON 文档。Elasticsearch 是用 Java 开发的。

# Elasticsearch 是用来做什么的？

**Elasticsearch** 允许您快速、近乎实时地存储、搜索和分析海量数据，并在几毫秒内给出答案。它能够实现快速搜索响应，因为它不是直接搜索文本，而是搜索索引。

# 弹性搜索—一些基本概念

*   索引—不同类型的文档和文档属性的集合。例如，文档集可以包含社交网络应用的数据。
*   类型/映射——共享同一索引中一组公共字段的文档集合。例如，索引包含社交网络应用的数据；可以有一种特定类型的用户简档数据、另一种类型的消息数据以及另一种类型的评论数据。
*   文档——以特定方式在 JSON 格式中定义的字段集合。每个文档都属于一种类型，并驻留在索引中。每个文档都有一个唯一的标识符，称为 UID。
*   字段-弹性搜索字段可以包含多个相同类型的值(本质上是一个列表)。另一方面，在 SQL 中，一列只能包含一个上述类型的值。

# 使用 Django 的 Elasticsearch

**安装和配置:**

*   安装 Django Elasticsearch DSL: $pip 安装 django-elasticsearch-dsl
*   然后将`django_elasticsearch_dsl`添加到 INSTALLED_APPS 中
*   您必须在 django 设置中定义`ELASTICSEARCH_DSL`。
*   例如:

```
ELASTICSEARCH_DSL={
    'default': {
        'hosts': 'localhost:9200'
    },
}
```

**向索引申报数据:**

*   然后对于一个模型:

```
*# models.py*

class Category(models.Model):
    name = models.CharField(max_length=30)
    desc = models.CharField(max_length=100, blank=True)def __str__(self):
    return '%s' % (self.name)
```

*   为了让这个模型与 Elasticsearch 一起工作，创建一个`django_elasticsearch_dsl.Document`的子类，在`Document`类中创建一个`class Index`来定义你的 Elasticsearch 索引、名称、设置等，最后使用`registry.register_document`装饰器注册这个类。需要在你的 app 目录的`documents.py`中定义`Document`类。

```
*# documents.py*

**from** **django_elasticsearch_dsl** **import** Document
**from** **django_elasticsearch_dsl.registries** **import** registry
**from** **.models** **import** Category

@registry.register_document
class CategoryDocument(Document):
    class Index:
        name = 'category' settings = {
        'number_of_shards': 1,
        'number_of_replicas': 0
    } class Django:
         model = Category fields = [
             'name',
             'desc',
         ]
```

**填充:**

*   要创建和填充 Elasticsearch 索引和映射，请使用 search_index 命令:$ python manage . py search _ index-rebuild
*   有关更多帮助，请使用$ python manage . py search _ index-help 命令
*   现在，当你做这样的事情时:

```
category = Category(
    name="Computer and Accessories",
    desc="abc desc"
)
category.save()
```

*   该对象也将保存在 Elasticsearch 中(使用信号处理器)。

**搜索:**

*   要获得 elasticsearch-dsl-py 搜索实例，请使用:

```
s = CategoryDocument.search().filter("term", name="computer")

*# or*

s = CategoryDocument.search().query("match", description="abc")

**for** hit **in** s:
    **print**(
        "Category name : {}, description {}".format(hit.name, hit.desc)
    )
```

*   要将 elastic search 结果转换成一个真正的 django queryset，只需注意这需要一个 SQL 请求来检索带有 elastic search 查询返回的 id 的模型实例。

```
s = CategoryDocument.search().filter("term", name="computer")[:30]
qs = s.to_queryset()
*# qs is just a django queryset and it is called with order_by to keep*
*# the same order as the elasticsearch result.*
**for** cat **in** qs:
    **print**(cat.name)
```

# 谁用 Elasticsearch？

*   易贝——凭借无数利用 Elasticsearch 作为主干的业务关键型文本搜索和分析用例，易贝创建了一个定制的“Elasticsearch 即服务”平台，允许在其内部基于 OpenStack 的云平台上轻松配置 Elasticsearch 集群。
*   脸书使用 Elasticsearch 已经 3 年多了，已经从一个简单的企业搜索发展到跨多个集群的 40 多种工具，每天有 6000 多万次查询，并且还在增长。
*   优步——elastic search 在优步的 Marketplace Dynamics 核心数据系统中发挥着关键作用，它可以实时汇总业务指标以控制关键的市场行为，如动态(激增)定价、供应定位和评估整体市场诊断。
*   Github 使用 Elasticsearch 索引超过 800 万个代码库，以及关键事件数据。
*   微软——使用 Elasticsearch 支持各种产品的搜索和分析，包括 MSN、微软社交监听和 Azure Search。
*   just Eat-elastic search 提高了配送半径的准确性，因为它可以用于定义更复杂的配送路线，并在餐厅做出改变时提供实时更新。

感谢阅读。如果你发现这篇文章有用，别忘了**鼓掌**和**与你的朋友和同事分享。:)如果你有任何问题，请随时联系我。
**与我接通👉**[**LinkedIn**](https://linkedin.com/in/hiteshmishra708)**，**[**Github**](https://github.com/hiteshmishra708)**:)****