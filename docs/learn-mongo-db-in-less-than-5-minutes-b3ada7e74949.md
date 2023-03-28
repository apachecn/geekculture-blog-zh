# 在不到 5 分钟的时间内学习 Mongo DB

> 原文：<https://medium.com/geekculture/learn-mongo-db-in-less-than-5-minutes-b3ada7e74949?source=collection_archive---------49----------------------->

![](img/84610a130f213316531a128dfcb02e9f.png)

当你在学习 Node.js 的时候，学习数据库是必须的。在这种情况下，MongoDB 和 Mongoose 将成为您最好的朋友。我们可以说 Mongo DB 是一个 NoSQL 数据库，这意味着它是一个面向文档的数据库，所有数据都可以存储在一个文档文件中——在本例中，将是一个 JSON 文档。结构非常简单:在 MongoDB 中，您可以在每个数据库上创建一个集合。每个集合都有一个键和值。由于这些输入，NoSQL 数据库很容易进行更改。

如果您熟悉 JSON，您会知道可以向对象添加更多的键和值:

```
{
    field1: value1,
    field2: value2,
    field3: value3,   
    field4:{
        fiel4-1: value4-1,
        field4-2: value4-2
    }
}
```

你可能会想，如果我不给对象任何 id 呢？不要担心，MongoDB 会为您添加 id，以防您没有添加。

## 明白了！但是我们为什么需要猫鼬呢？

Mongoose 是一个 JavaScript 库，它基于一个模式定义了一个特定的模型——这是一个 JSON 结构，包含文件中的属性信息，比如验证。

但是首先，我们需要用 connect()方法连接我们的数据库。connect()有一个强制参数，它是到 MongoDB 数据库或 uri 的链接。之后，我们需要使用 listen()方法来运行服务器。

定义模式后，我们可以创建一个模型，并将其分配给 MongoDB 上的一个文档文件。多亏了 Mongoose，您可以保存、删除或验证您的数据——我们也可以使用 express-validate 来验证我们的数据——使用 MongoDB 的异步功能。这是一个如何定义模式的示例:

```
**const** testSchema = mongoose.Schema({
    name: String,
})
```

模式中可以定义各种数据:

*   线
*   数字
*   布尔代数学体系的
*   缓冲区(二进制类型)
*   日期
*   排列
*   模式。类型. ObjectIdString(十六进制)
*   模式。类型。混合(任何数据)

我们可以使用 done()在异步操作完成后继续。它可以被称为成功完成(null，data)，也可以被称为错误完成(err)。

# **管理我们的数据库**

让我们看看在管理数据时可以使用的一些查询:

*   **在我们的数据库中查询**:我们可以使用 find()来查询集合内部。

```
db.collection.find()
```

当我们想要**查阅**我们使用的集合中具有特定值的特定文档时

```
db.collection.find({field:"value"})
```

*   创建模型的一些实例:Create()将一个对象数组作为第一个参数，并将所有对象保存在数据库中。
*   **在我们的数据库中找到**:

我们可以使用 find()，它接受一个对象作为第一个参数和一个回调。这将返回一个匹配数组。

我们也可以使用 findOne()，它将返回一个匹配的文档。

也可以使用 findById()来查找字母数字键。

*   **在我们的数据库中插入**:我们可以使用一些方法。首先，我们可以使用 insert()

```
db.collection.insert( { _id: 10, type: "misc", item: "book", qty: 5 
} )
```

*   **在我们的数据库中更新**:我们可以使用 Update()方法，并且我们需要在其中添加 upsert 标志。我们需要指出我们想要更新的对象的 id，否则这将是数据库中的一个新元素。

```
db.collection.update( 
{ _id: 10, type: "misc", item: "book", qty: 5 },
{upsert:true}
 )
```

您还可以使用标志{ multi: true }来更新多个对象。

*   您可以使用 save()方法**插入**一个文档。

```
db.collection.save(function(err, data) { _id: 10, type: "misc", item: "book", qty: 5 
} )
```

*   您还可以使用 insert one()方法**插入一个**文档

```
db.collection.insertOne( { _id: 10, type: "misc", item: "book", qty: 5 
} )
```

*   要**移除**一个文档，你可以使用 remove()或 drop()方法:

remove()删除符合特定条件的文档，或者传递一个空文档来删除当前文档。

drop()删除所有的文档、id 并删除集合。这种方法效率更高。

```
db.collection.remove( { type: "misc"} )db.collection.drop( { type: "misc" } )
```