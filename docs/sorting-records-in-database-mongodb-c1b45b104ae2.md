# 对数据库中的记录进行排序(MongoDB)

> 原文：<https://medium.com/geekculture/sorting-records-in-database-mongodb-c1b45b104ae2?source=collection_archive---------26----------------------->

![](img/36c1b3ba7d5408efd8e3e64f8f83b5d0.png)

在本文中，我将解释如何在 MongoDB 中对记录(用户)进行排序。MongoDB 中有一个函数，名为$sort 但是在我的大学里，我们不用这个函数。所以我必须为此找到一个方法。开始吧！

首先，我们需要知道我们的记录数量。在 MongoDB 中，我们有 find()和 count 函数。使用这两个，我们基本上可以找到这样的计数:

```
const count = await User.find().count();
```

用户是数据库的名称。然后我们用 let 为用户创建一个数组。意思是，我们可以随时改变它。我们将把数据库中的用户放到这个数组中。为此，我们需要一个 For 循环。在循环中，我们将使用 MongoDB 中的 findOne()函数。

```
for (let i = 0; i < count; i++) {users[i] = await User.findOne({ id: i })}
```

我将使用递归冒泡排序，因为它易于理解和编码。对于这个算法，我们需要一个临时变量。当我们对其他记录进行排序时，temp 将存储一个记录。

```
for (k = 0; k < count - 1; k++) {for (l = 0; l < count - k - 1; l++) {if (users[l].point < users[l + 1].point) {temp = users[l]users[l] = users[l + 1];users[l + 1] = temp}}}
```

在这段代码中，最重要的部分是‘如果’。如果一个用户的点数比另一个小，我们存储一个小记录，等于一个大记录，并用 temp 改变它。这叫做交换，仅此而已。

现在，我选择在数组中存储用户名、点数和 id。为此，我编写了以下代码:

```
let names = []let points = []let _ids = []for (let i = 0; i < count; i++) {names[i] = users[i].namepoints[i] = users[i].point_ids[i] = users[i]._id}
```

然后带着

```
return *res*.json({data: {_ids,names,points}})
```

就是这样！当我们用 npm start 启动服务器并转到 Postman 时，我们将在用户信息中看到 id、名称和点数。

本文以此结束。当然，对于这个例子，您可以只使用$sort 函数。但是我找不到没有$sort 的方法，所以我想解释一下怎么做。感谢阅读！