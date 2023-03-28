# 如何在 Mongo DB 中执行 Map Reduce

> 原文：<https://medium.com/geekculture/how-to-perform-map-reduce-in-mongo-db-abe35dc0ef78?source=collection_archive---------43----------------------->

![](img/292d024a743c523d6a41133f6625b449.png)

嘿，伙计们，希望你们在今天的文章中所做的事情。我们将对 mongo 数据库集群中的数据进行排序，因此我们可以通过多种方式来完成这项工作，但今天我们将使用 map-reduce 对 mongo 数据库集群中的数据进行排序。

在 map-reduce 中，我们创建了两个函数，一个用于映射数据，无处映射是指假设您有 1000 个文档(在 mongo 文档中是指我们从用户那里获得的每个数据) 假设在该文档中，我们有许多具有相同客户 id 的客户，但他们每次都购买新产品，并且每次客户购买客户时都会创建一个新条目。现在，我们希望制作一个以客户 id 命名的列表，其中包含该客户 id 花费的所有价格，一旦我们获得每个客户的价格列表，我们希望执行加法运算(这取决于您想要做什么 有了这个函数，我们只需编写函数)所以我们在这里编写 reduce 函数，它处理我们地图数据并给出我们想要的最终输出。

我希望你能了解什么是 map-reduce 程序，现在让我们开始实践，以便更好地理解。

首先，让我们使用下面的命令创建一个示例集合，它包含您想要的任何数据库中的名称顺序。

```
use [database name]
db.orders.insertMany([ { _id: 1, cust_id: “Ant O. Knee”, ord_date: new Date(“2020–03–01”), price: 25, items: [ { sku: “oranges”, qty: 5, price: 2.5 }, { sku: “apples”, qty: 5, price: 2.5 } ], status: “A” }, { _id: 2, cust_id: “Ant O. Knee”, ord_date: new Date(“2020–03–08”), price: 70, items: [ { sku: “oranges”, qty: 8, price: 2.5 }, { sku: “chocolates”, qty: 5, price: 10 } ], status: “A” }, { _id: 3, cust_id: “Busby Bee”, ord_date: new Date(“2020–03–08”), price: 50, items: [ { sku: “oranges”, qty: 10, price: 2.5 }, { sku: “pears”, qty: 10, price: 2.5 } ], status: “A” }, { _id: 4, cust_id: “Busby Bee”, ord_date: new Date(“2020–03–18”), price: 25, items: [ { sku: “oranges”, qty: 10, price: 2.5 } ], status: “A” }, { _id: 5, cust_id: “Busby Bee”, ord_date: new Date(“2020–03–19”), price: 50, items: [ { sku: “chocolates”, qty: 5, price: 10 } ], status: “A”}, { _id: 6, cust_id: “Cam Elot”, ord_date: new Date(“2020–03–19”), price: 35, items: [ { sku: “carrots”, qty: 10, price: 1.0 }, { sku: “apples”, qty: 10, price: 2.5 } ], status: “A” }, { _id: 7, cust_id: “Cam Elot”, ord_date: new Date(“2020–03–20”), price: 25, items: [ { sku: “oranges”, qty: 10, price: 2.5 } ], status: “A” }, { _id: 8, cust_id: “Don Quis”, ord_date: new Date(“2020–03–20”), price: 75, items: [ { sku: “chocolates”, qty: 5, price: 10 }, { sku: “apples”, qty: 10, price: 2.5 } ], status: “A” }, { _id: 9, cust_id: “Don Quis”, ord_date: new Date(“2020–03–20”), price: 55, items: [ { sku: “carrots”, qty: 5, price: 1.0 }, { sku: “apples”, qty: 10, price: 2.5 }, { sku: “oranges”, qty: 10, price: 2.5 } ], status: “A” }, { _id: 10, cust_id: “Don Quis”, ord_date: new Date(“2020–03–23”), price: 25, items: [ { sku: “oranges”, qty: 10, price: 2.5 } ], status: “A” }])
```

在上面的命令中，顺序只是一个集合名称，你可以把它改成你想要的任何名称。

Mongo DB 中一些有助于一般操作的命令。

```
use dbname to change dbname
show collections to get collection in db
db.collectionnaame.find()=> To see all data of collection
```

现在我们准备制作映射器函数来映射数据。

```
var mapFunction1 = function() {emit(this.cust_id, this.price);}; 
```

在上面的映射器函数中，这是指特定文档的地址，并发出 take key: value 对，以在上面的命令中对集合中的项目进行分组，cust_id 是我们的键，price 是值。在后端运行该命令后，每个客户的数据将类似于{"cust_id":[price1，price 2，etc]}。

现在我们可以创建我们的 reducer 函数了，我们创建了一个函数，它带有两个参数 cust_id 和 price，这是我们在 mapper 函数中创建的，并返回数组中的一些元素。

```
var reduceFunction1 = function(keyCustId, valuesPrices) {return Array.sum(valuesPrices);};
```

现在我们可以从下面的命令中调用这个函数。

```
db.[collection name].mapReduce([mapfunction name],[mapfunction name],{ out: **[any collection name you want output want to be save]** })Eg:-db.orders.mapReduce(mapFunction1,reduceFunction1,{ out: **“map_reduce_example”** })
```

如果想知道更多关于上面的例子可以看看下面的 mongo 官方网站链接。

[](https://docs.mongodb.com/manual/tutorial/map-reduce-examples/) [## 地图缩小示例

### 聚合管道作为外壳中 Map-Reduce 的替代。收藏。MapReduce()方法是一个包装器…

docs.mongodb.com](https://docs.mongodb.com/manual/tutorial/map-reduce-examples/) 

伙计们，这个博客到此结束，我希望你们都喜欢它，并发现它内容丰富。如果有任何疑问，请随时联系我:)

> 伙计们关注我的博客，如果有任何评论，请告诉我，下次写博客时我会记住这些观点。如果想阅读更多这样的博客来了解我，这里是我的网站链接[](https://sites.google.com/view/adityvgupta/home)****。伙计们，请不要犹豫👏👏👏👏👏对于它(一个公开的秘密:你可以为一个帖子鼓掌 50 次，最棒的是，它不会花费你任何东西)，也可以随意分享。这对我真的很重要。****