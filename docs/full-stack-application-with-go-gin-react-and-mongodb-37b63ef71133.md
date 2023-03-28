# 具有 Go、Gin、React 和 MongoDB 的全栈应用程序

> 原文：<https://medium.com/geekculture/full-stack-application-with-go-gin-react-and-mongodb-37b63ef71133?source=collection_archive---------1----------------------->

嗨，最近怎么样！在本教程中，我们将介绍如何使用 go、ReactJS 和 MongoDB 构建一个可以在本地机器上运行的全栈应用程序。无论您是刚接触 Go、ReactJS 或 MongoDB，还是刚接触 web 开发，本教程都是一个很好的起点，可以让您了解这些组件，并了解它们是如何组合在一起的。对于我们的项目，我们将创建一个管理餐馆订单的应用程序。

# 去

我们将使用 Go 作为我们的后端 web 服务器，我们将建立一个具有基本 CRUD(创建，读取，更新，删除)功能的 API，我们的前端应用程序可以与之接口。

我们将使用 Gin 框架，关于该框架的更多信息可以在这里找到。

如果你还没有下载围棋，那就去围棋网站[这里](https://golang.org/)下载吧。如果你想了解更多关于围棋的知识，围棋官方网站也提供了教程。 [W3Schools](https://www.w3schools.com/go/index.php) 也有关于围棋的精彩章节。

# 反应

对于我们的前端应用程序，我们将使用 React，这是一个用于构建用户界面的很好的 JavaScript 库。

要使用 React，你需要下载[节点](https://nodejs.org/en/download/)以及 [NPM](https://www.npmjs.com/package/download) 。

如果你想了解更多关于 React 的知识，请点击这里查看 W3Schools 提供的 React 上的片段。

# MongoDB

最后，我们将使用 [MongoDB](https://cloud.mongodb.com/) 作为我们的数据库。MongoDB 是一个面向文档的数据库程序，这意味着它不使用 SQL，而是使用 JSON 来存储信息。它还有一个免费层(我们将使用它)，所以对于任何项目来说，它都是一个非常好的数据库选择。

MongoDB 不需要任何下载，但是我推荐下载 [Postman](https://www.postman.com/) ，这是一个测试我们 API 的便捷工具。

# 设置数据库

好了，现在你已经熟悉了我们的堆栈，并且已经下载了所有必要的工具，让我们来设置我们的数据库吧！

首先前往 [MongoDB](https://cloud.mongodb.com/) 并登录/注册。一旦你进入，你的主屏幕应该是这样的:

![](img/9fcb25a817bf0f4e3b887f1e6d8088a1.png)

点击“构建数据库”并选择“共享”选项，创建一个新的数据库。

![](img/d79c113ac3d6b34bcc644a1dda0786e7.png)

保留默认选项并选择“创建集群”

![](img/e02d2b5bff10e3373a167fcbc41a63c8.png)

创建集群后，单击“Connect”按钮。

![](img/352f6d700bde004265107b781cf41a83.png)

选择您的 IP 地址并创建用户名和密码。然后点击“选择连接方式”

![](img/b794002cfb000b943f05191e817d24fb.png)

选择“连接应用程序”选项。

![](img/efb416c14e94dfd8eb901554efb0d71d.png)

保存连接字符串并单击“关闭”

![](img/7e9209cfd119aa15558120194cadcfbb.png)

连接字符串是我们的后端 API 将与数据库通信的 URL 端点。

用您的密码替换<password>,用您的集群名替换 myFirstDatabase。</password>

```
mongodb+srv://admin:mypassword@cluster0.56rq1f.mongodb.net/Cluster0?retryWrites=true&w=majority
```

现在，我们已经完成了数据库部分，可以开始应用程序的后端部分了。

# 后端 API

既然我们已经建立了数据库并准备好了，让我们来建立我们的 API。首先，我们要创建一个文件夹来存放我们所有的服务器文件。打开您的终端，导航到您的项目文件夹，创建一个名为“server”的新文件夹，并输入它。

![](img/5f563522dc67f2d2203f43f83cfaada8.png)

接下来，我们将创建我们的 mod 文件。这将包含我们服务器的所有配置。

键入:`go mod init server`并按回车键。

这将在您的文件夹中创建一个名为 server 的包，并创建一个 go.mod 文件。我们需要添加 MongoDB 和 Gin 的包，我们通过打开 go.mod 并添加以下代码行来完成此操作:

```
require (
    github.com/gin-gonic/gin v1.7.3
    github.com/go-playground/validator/v10 v10.4.1
    github.com/joho/godotenv v1.3.0
    go.mongodb.org/mongo-driver v1.7.1
)
```

您的 go.mod 文件应该如下所示:

接下来，在服务器目录中创建一个名为 main.go 的文件，并向其中添加以下代码:

在 main.go 中，我们首先获得我们的进口。如你所见，我们正在导入 gin 框架以及一个名为 routes 的包，我们将在后面创建它。在 main 函数中，我们首先设置端口，然后设置路由器。在这个函数中，我们还有不同的端点，我们的 API 将不得不执行 CRUD 操作。

在我们继续之前，让我们创建我们的。环境文件夹。这将保存我们的安全信息。创建一个名为。env 并添加以下代码行，其中 MONGODB_URL=包含前面的 MONGODB 端点:

```
PORT=5000
MONGODB_URL=mongodb+srv://admin:Password@cluster0.655rf.mo
ngodb.net/Cluster0?retryWrites=true&w=majority
```

如果您要将您的更改推送到 GitHub，请确保添加。env 文件到您的。gitignore 文件。

接下来，我们将为餐馆订单创建数据模型。在服务器目录中，创建一个名为 models 的新目录，并在其中创建一个名为 order.go 的文件，然后向其中添加以下代码:

在 order.go 中，第一行是“包模型”。这将创建包，以便我们可以在代码的其他区域引用它。接下来，我们将导入 bson/primitive，以便为每个订单创建一个 ObjectID 惟一 ID。之后，我们有了订单的模式。如您所见，每个订单都有一个惟一的 id、一道菜、一个价格、一个服务器和一张桌子。

创建好模式后，让我们创建路线。回到我们的服务器目录，创建另一个名为 routes 的目录。还记得我们如何在 main.go 中导入服务器/路由吗？这就是了。继续创建一个名为 connections.go 的文件，它将处理我们到数据库的连接，并输入以下代码:

在 connections.go 中，与所有其他文件一样，我们有将在该文件中使用的导入。接下来是 DBInstance 函数，它接受存储在。env 文件并创建一个连接。另一个函数 OpenCollection 接受一个集合名称并打开一个集合。

我们将在 routes 文件夹中创建的下一个文件是 orders.go。该文件将保存订单模型的所有函数，这些函数处理我们在 main.go 中设置的各种端点的 api 请求。

在 orders.go 中，首先我们将添加导入并创建几个变量:

```
package routesimport (
    "context"
    "fmt"
    "net/http"
    "time"

    "server/models"
    "github.com/go-playground/validator/v10"
    "go.mongodb.org/mongo-driver/mongo"
    "go.mongodb.org/mongo-driver/bson/primitive"
    "go.mongodb.org/mongo-driver/bson"
    "github.com/gin-gonic/gin"
)var validate = validator.New()
var orderCollection *mongo.Collection = OpenCollection(Client, "orders")
```

变量 validate 允许我们确保在创建或更新订单时收到的订单数据是正确的，orderCollection 为我们打开了一个新的集合，称为“orders”。

接下来，我们将创建一个名为 AddOrder 的函数来处理新订单的创建:

```
//add an order
func AddOrder(c *gin.Context) {
    var ctx, cancel = context.WithTimeout(context.Background(), 100*time.Second)
    var order models.Order

    *if* err := c.BindJSON(&order); err != nil {
        c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
        *return* }

    validationErr := validate.Struct(order)
    *if* validationErr != nil {
         c.JSON(http.StatusBadRequest, gin.H{"error": validationErr.Error()})
         *return* } order.ID = primitive.NewObjectID()

    result, insertErr := orderCollection.InsertOne(ctx, order)
    *if* insertErr != nil {
        msg := fmt.Sprintf("order item was not created")
        c.JSON(http.StatusInternalServerError, gin.H{"error": msg})
        *return* }

    *defer* cancel()
    c.JSON(http.StatusOK, result)
}
```

在 AddOrder 中(附注:首字母大写的函数被认为是公共的，否则它们被认为是私有的)，我们创建了一个超时的上下文。接下来，我们创建订单变量，并用用户提供的数据填充它。接下来，我们验证数据，然后为订单创建唯一的 id。最后，我们将新的订单对象插入数据库并返回结果。

我们要创建的下一个函数是 GetOrders，它将返回集合中的所有订单:

```
//get all orders
func GetOrders(c *gin.Context){
    var ctx, cancel = context.WithTimeout(context.Background(), 100*time.Second)
    var orders []bson.M cursor, err := orderCollection.Find(ctx, bson.M{})
    *if* err != nil {
        c.JSON(http.StatusInternalServerError, gin.H{"error": err.Error()})
        *return* } *if* err = cursor.All(ctx, &orders); err != nil {
        c.JSON(http.StatusInternalServerError, gin.H{"error": err.Error()})
        *return* }

    *defer* cancel()

    fmt.Println(orders) c.JSON(http.StatusOK, orders)
}
```

这个函数非常简单，我们创建一个订单数组，并使用 orderCollection.Find 从数据库中的数据填充这些订单。像 AddOrder 函数和我们将要创建的所有其他函数一样，我们创建一个超时的上下文，并在每一步进行错误检查。

下一个函数是 GetOrdersByWaiter，这个函数建立在 GetOrders 的基础上。它接受一个服务员的名字，并返回该服务员的所有订单。

```
//get all orders by the waiter's name
func GetOrdersByWaiter(c *gin.Context){

    waiter := c.Params.ByName("waiter")
    var ctx, cancel = context.WithTimeout(context.Background(), 100*time.Second)
    var orders []bson.M cursor, err := orderCollection.Find(ctx, bson.M{"server": waiter})
    *if* err != nil {
        c.JSON(http.StatusInternalServerError, gin.H{"error": err.Error()})
        *return* } *if* err = cursor.All(ctx, &orders); err != nil {
        c.JSON(http.StatusInternalServerError, gin.H{"error": err.Error()})
        *return* } *defer* cancel() fmt.Println(orders) c.JSON(http.StatusOK, orders)
}
```

如您所见，除了几个例外，它与前面的函数非常相似。首先，我们接受一个名为 waiter 的参数，其次，我们将该参数用作 orderCollection.Find 中的过滤器。

我们的最后一个 GET 函数是 GetOrderById，它将接受一个订单 Id 并返回一个订单。

```
//get an order by its id
func GetOrderById(c *gin.Context){
    orderID := c.Params.ByName("id")
    docID, _ := primitive.ObjectIDFromHex(orderID) var ctx, cancel = context.WithTimeout(context.Background(), 100*time.Second) var order bson.M *if* err := orderCollection.FindOne(ctx, bson.M{"_id": docID}).Decode(&order); err != nil {
        c.JSON(http.StatusInternalServerError, gin.H{"error": err.Error()})
        *return* } *defer* cancel() fmt.Println(order) c.JSON(http.StatusOK, order)
}
```

我们在 GetOrderById 中做的第一件事是获取 Id 参数并将其转换为 ObjectID 对象。然后我们使用 orderCollection。FindOne 查找与订单 id 匹配的订单。

接下来的几个函数将是我们的 PUT 函数，它将处理更新订单。我们将实现的第一个函数是 UpdateWaiter，它接受一个 id 作为参数，以及一个简单的用于服务员的 json 对象，并用订单 id 替换找到的订单，以获得新服务员的姓名。

```
//update a waiter's name for an order
func UpdateWaiter(c *gin.Context){ orderID := c.Params.ByName("id")
    docID, _ := primitive.ObjectIDFromHex(orderID) var ctx, cancel = context.WithTimeout(context.Background(), 100*time.Second) type Waiter struct {
        Server      **string*             `json:"server"`
    } var waiter Waiter *if* err := c.BindJSON(&waiter); err != nil {
        c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
        *return* } result, err := orderCollection.UpdateOne(ctx, bson.M{"_id": docID},
        bson.D{
            {"$set", bson.D{{"server", waiter.Server}}},
        },
    ) *if* err != nil {
        c.JSON(http.StatusInternalServerError, gin.H{"error": err.Error()})
        *return* } *defer* cancel() c.JSON(http.StatusOK, result.ModifiedCount)
}
```

在 UpdateWaiter 中，我们首先获取 id 参数，并将其转换为 ObjectID 对象。然后我们为我们的服务员创建一个新的结构，并用用户提供的数据填充它。之后，我们使用 orderCollection。UpdateOne 用服务员的名字更新订单。

我们要创建的下一个函数叫做 UpdateOrder。这与前面的函数类似，只是它将接受一个 order 对象并用这个新订单替换现有订单。

```
//update the order
func UpdateOrder(c *gin.Context){
    orderID := c.Params.ByName("id")
    docID, _ := primitive.ObjectIDFromHex(orderID) var ctx, cancel = context.WithTimeout(context.Background(), 100*time.Second) var order models.Order
    *if* err := c.BindJSON(&order); err != nil {
        c.JSON(http.StatusBadRequest, gin.H{"error": err.Error()})
        *return* } validationErr := validate.Struct(order)
    *if* validationErr != nil {
        c.JSON(http.StatusBadRequest, gin.H{"error": validationErr.Error()})
        *return* } result, err := orderCollection.ReplaceOne(
        ctx,
        bson.M{"_id": docID},
        bson.M{
            "dish":  order.Dish,
            "price": order.Price,
            "server": order.Server,
            "table": order.Table,
        },
    ) *if* err != nil {
        c.JSON(http.StatusInternalServerError, gin.H{"error": err.Error()})
        *return* }

    *defer* cancel() c.JSON(http.StatusOK, result.ModifiedCount)
}
```

在 UpdateOrder 中，我们接受 id 作为参数，然后接受订单数据进行验证。之后，我们使用 orderCollection。ReplaceOne 用该 id 的新订单替换订单。

我们将创建的最后一个函数是 DeleteOrder 函数。这将接受一个 id 作为参数，并删除与该 id 匹配的订单。

```
//delete an order given the id
func DeleteOrder(c * gin.Context){
    orderID := c.Params.ByName("id")
    docID, _ := primitive.ObjectIDFromHex(orderID) var ctx, cancel = context.WithTimeout(context.Background(), 100*time.Second) result, err := orderCollection.DeleteOne(ctx, bson.M{"_id": docID})
    *if* err != nil {
        c.JSON(http.StatusInternalServerError, gin.H{"error": err.Error()})
        *return* } *defer* cancel() c.JSON(http.StatusOK, result.DeletedCount)
}
```

如您所见，我们正在使用 orderCollection。DeleteOne 删除与 id 匹配的订单。

完整的文件如下所示:

我们已经创建了所有文件，我们的文件结构应该如下所示:

![](img/6df318c0b7f90524ff9e50a1d5191f62.png)

现在我们有了所有的文件，我们可以构建并运行我们的服务器了。首先导航到您的服务器目录，并输入以下命令:

去 mod 整理
去 build main.go
去 run main.go

“go mod tidy”添加导入，“go build main.go”构建项目，“go run main.go”运行项目。您的输出应该如下所示:

![](img/4d97b77286b95450ccd9af31c77de290.png)

为了测试这些 url 端点，我们可以使用 postman。

让我们创建几个订单:

![](img/1ff9ec7888d6ca089c59e0915f5f1ebc.png)

Creating an order of french fries for waiter: john

![](img/23847ec51c88b6462cfcbbda22f3a123.png)

Creating an order of chicken nuggets for waiter: Dave

![](img/c3d22fcfc343b60160917546dc2c003c.png)

Creating another order for dave

现在让我们来看看我们创建的订单。首先让我们得到所有的订单:

![](img/a3b349fce13261e1142c1030dfd03125.png)

All the orders

接下来让我们为服务员戴夫点菜:

![](img/5eabb32b5f9c03355d3075ad44d123de.png)

All of Dave’s orders

现在让我们只得到一个订单:

![](img/633411afe4a7741fd93ead20286a9da1.png)

让我们测试一下我们的更新端点。让我们更新我们创建的鸡块订单，然后为它分配一个不同的服务员。出于演示的目的，我们使用了两种不同的操作，尽管我们可以只使用一个更新订单函数。

![](img/55aa25404889b12950cbfbf76637fe23.png)

Updating the chicken nuggets order

![](img/6339dc13f292826516566f63eda8324b.png)

updating the order to have a new waiter: luke

![](img/e8a25f06b3c14ce9fc54e5ff50e1dd99.png)

Test our changes using the get order endpoint

最后，让我们测试我们的删除端点。

![](img/170ecae36f4567ef63b37005f42fec11.png)

Deleting the order for which the waiter is Luke

![](img/59c62c9d63d267315ab28368b40da405.png)

Testing to make sure that the entry was deleted.

太好了！既然我们已经设置了 web 服务器，我们就可以实现我们的前端应用程序了。

# 前端

对于前端，我们将使用 React，这是一个很好的 UI 框架。首先，让我们导航到主项目文件夹，通过键入“npx create-react-app frontend”创建一个新的 react 项目，这将创建一个名为 frontend 的新应用程序。

创建应用程序后，导航到前端目录并输入以下命令

npm 安装 axios
npm 安装 react-bootstrap @ next bootstrap @ 5 . 1 . 0

我们将使用 axios 来处理我们对服务器的请求，react-bootstrap 用于我们的 UI 样式。

在我们的/src 目录中创建一个名为“components”的新文件夹，并在该文件夹中创建两个新文件 orders.components.js 和 single-order.component.js

在 single-order.components.js 中，添加以下代码:

```
*import* React, {useState, useEffect} *from* 'react';
*import* 'bootstrap/dist/css/bootstrap.css';
*import* {Button, Card, Row, Col} *from* 'react-bootstrap'*const* Order = ({orderData, setChangeWaiter, deleteSingleOrder, setChangeOrder}) => {
    return (
    )
}export default Order
```

我们的订单组件将接收订单数据、删除订单的函数和准备订单以更新服务员或更改订单的函数。我们将回头讨论它，但现在将以下代码添加到 orders.components.js 中:

```
*import* React, {useState, useEffect} *from* 'react';
*import* axios *from* "axios";
*import* {Button, Form, Container, Modal } *from* 'react-bootstrap'
*import* Order *from* './single-order.component';*const* Orders = () => {
    return (
    )
}export default Orders
```

该组件将在我们的网页上显示所有订单。返回一个目录，将 App.js 更改为以下内容，以便它调用 Orders 组件:

回到 single-order.component.js，在 return 语句之后，添加这两个函数。

```
*function* changeWaiter(){
    setChangeWaiter({
        "change": true,
        "id": orderData._id
    })
}*function* changeOrder(){
    setChangeOrder({
        "change": true,
        "id": orderData._id
    })
}
```

这将设置 setChangeWaiter 和 setChangeOrder 的值，并提醒订单组件显示更改数据的弹出窗口。在 return 语句中，添加以下代码:

```
<Card>
    <Row>
        <Col>Dish:*{* orderData !== undefined && orderData.dish*}*</Col>
        <Col>Server:*{* orderData !== undefined && orderData.server </Col>
        <Col>Table:*{* orderData !== undefined && orderData.table*}*</Col>
        <Col>Price: $*{*orderData !== undefined && orderData.price*}*</Col>
        <Col><Button *onClick*=*{*() => deleteSingleOrder(orderData._id)*}*>delete order</Button></Col>
        <Col><Button *onClick*=*{*() => changeWaiter()*}*>change waiter</Button></Col>
        <Col><Button *onClick*=*{*() => changeOrder()*}*>change order</Button></Col>
    </Row>
</Card>
```

这将在网页中显示有关订单的信息，以及删除、更改和更新服务员的按钮。

您完成的文件应该如下所示:

现在转到 orders.component.js 文件，在组件的开头添加以下代码:

```
*const* [orders, setOrders] = useState([])
*const* [refreshData, setRefreshData] = useState(false)*const* [changeOrder, setChangeOrder] = useState({"change": false, "id": 0})
*const* [changeWaiter, setChangeWaiter] = useState({"change": false, "id": 0})
*const* [newWaiterName, setNewWaiterName] = useState("")*const* [addNewOrder, setAddNewOrder] = useState(false)
*const* [newOrder, setNewOrder] = useState({"dish": "", "server": "", "table": 0, "price": 0})
```

这些将是我们用来控制添加、更新、显示和删除订单的变量。每个变量都有一个 setter，调用它时会刷新页面。

在 return 语句后添加以下函数，这些函数将使用 axios 向服务器提交对我们各种功能的请求:

```
//changes the waiter
*function* changeWaiterForOrder(){
    changeWaiter.change = false
    *var* url = "http://localhost:5000/waiter/update/" + changeWaiter.id
    axios.put(url, {
        "server": newWaiterName
    }).then(response => {
        console.log(response.status)
        *if*(response.status == 200){
            setRefreshData(true)
        }
    })
}//changes the order
*function* changeSingleOrder(){
    changeOrder.change = false;
    *var* url = "http://localhost:5000/order/update/" + changeOrder.id
    axios.put(url, newOrder)
        .then(response => {
        *if*(response.status == 200){
            setRefreshData(true)
        }
    })
}//creates a new order
*function* addSingleOrder(){
    setAddNewOrder(false)
    *var* url = "http://localhost:5000/order/create"
    axios.post(url, {
        "server": newOrder.server,
        "dish": newOrder.dish,
        "table": newOrder.table,
        "price": parseFloat(newOrder.price)
    }).then(response => {
        *if*(response.status == 200){
            setRefreshData(true)
        }
    })
}//gets all the orders
*function* getAllOrders(){
    *var* url = "http://localhost:5000/orders"
    axios.get(url, {
        responseType: 'json'
    }).then(response => {
        *if*(response.status == 200){
            setOrders(response.data)
        }
    })
}//deletes a single order
*function* deleteSingleOrder(id){
    *var* url = "http://localhost:5000/order/delete/" + id
    axios.delete(url, {
    }).then(response => {
        *if*(response.status == 200){
            setRefreshData(true)
        }
    })
}
```

然后在 return 语句的正上方添加以下内容:

```
//gets run at initial loadup
useEffect(() => {
    getAllOrders();
}, [])//refreshes the page
*if*(refreshData){
    setRefreshData(false);
    getAllOrders();
}
```

UseEffect 最初被调用以获取所有数据，由于它在末尾有[]，所以它将只被调用一次。然后，if 语句在每次刷新时检查订单是否需要通过 refreshData 进行更新。每次成功完成对服务器的调用时，都会调用 SetRefreshData()。

现在，在 return 语句中添加以下代码:

```
<div>
    *{*/* add new order button */*}* <Container>
         <Button *onClick*=*{*() => setAddNewOrder(true)*}*>Add new order</Button>
    </Container> *{*/* list all current orders */*}* <Container>
        *{*orders != null && orders.map((order, i) => (
            <Order *orderData*=*{*order*}* *deleteSingleOrder*=*{*deleteSingleOrder*}* *setChangeWaiter*=*{*setChangeWaiter*}* *setChangeOrder*=*{*setChangeOrder*}*/>
        ))*}* </Container> *{*/* popup for adding a new order */*}* <Modal *show*=*{*addNewOrder*}* *onHide*=*{*() => setAddNewOrder(false)*}* *centered*>
        <Modal.Header *closeButton*>
            <Modal.Title>Add Order</Modal.Title>
        </Modal.Header>
        <Modal.Body>
            <Form.Group>
                <Form.Label >dish</Form.Label>
                <Form.Control *onChange*=*{*(event) => {newOrder.dish = event.target.value}*}*/>
                <Form.Label>waiter</Form.Label>
                <Form.Control *onChange*=*{*(event) => {newOrder.server = event.target.value}*}*/>
                <Form.Label >table</Form.Label>
                <Form.Control *onChange*=*{*(event) => {newOrder.table = event.target.value}*}*/>
                <Form.Label >price</Form.Label>
                <Form.Control *type*="number" *onChange*=*{*(event) => {newOrder.price = event.target.value}*}*/>
            </Form.Group>
        <Button *onClick*=*{*() => addSingleOrder()*}*>Add</Button>
        <Button *onClick*=*{*() => setAddNewOrder(false)*}*>Cancel</Button>
     </Modal.Body>
     </Modal> *{*/* popup for changing a waiter */*}* <Modal *show*=*{*changeWaiter.change*}* *onHide*=*{*() => setChangeWaiter({"change": false, "id": 0})*}* *centered*>
        <Modal.Header *closeButton*>
            <Modal.Title>Change Waiter</Modal.Title>
        </Modal.Header>
        <Modal.Body>
            <Form.Group>
                <Form.Label >new waiter</Form.Label>
                <Form.Control *onChange*=*{*(event) => {setNewWaiterName(event.target.value)}*}*/>
            </Form.Group>
            <Button *onClick*=*{*() => changeWaiterForOrder()*}*>Change</Button>
            <Button *onClick*=*{*() => setChangeWaiter({"change": false, "id": 0})*}*>Cancel</Button>
        </Modal.Body>
    </Modal> *{*/* popup for changing an order */*}* <Modal *show*=*{*changeOrder.change*}* *onHide*=*{*() => setChangeOrder({"change": false, "id": 0})*}* *centered*>
        <Modal.Header *closeButton*>
            <Modal.Title>Change Order</Modal.Title>
        </Modal.Header>
        <Modal.Body>
            <Form.Group>
                <Form.Label >dish</Form.Label>
                <Form.Control *onChange*=*{*(event) => {newOrder.dish = event.target.value}*}*/>
                <Form.Label>waiter</Form.Label>
                <Form.Control *onChange*=*{*(event) => {newOrder.server = event.target.value}*}*/>
                <Form.Label >table</Form.Label>
                <Form.Control *onChange*=*{*(event) => {newOrder.table = event.target.value}*}*/>
                <Form.Label >price</Form.Label>
                <Form.Control *type*="number" *onChange*=*{*(event) => {newOrder.price = parseFloat(event.target.value)}*}*/>
            </Form.Group>
            <Button *onClick*=*{*() => changeSingleOrder()*}*>Change</Button>
            <Button *onClick*=*{*() => setChangeOrder({"change": false, "id": 0})*}*>Cancel</Button>
        </Modal.Body>
    </Modal>
</div>
```

您完成的代码应该如下所示:

现在我们已经有了所有的代码，在你的终端中进入/frontend 目录并运行“npm start”。这将在你的浏览器中运行网页，你应该能够运行完成的项目。

![](img/8be01afe8b8dbdcc7af633408caaa531.png)

adding an order

![](img/602cff287d8a9008b23cec4ba68ffcf7.png)

viewing all open orders

![](img/80965e4c7ec58eca6fa13d4c2017bc8a.png)

Changing the waiter

感谢阅读这篇文章！带有完整代码的 GitHub repo 位于这里:[https://github.com/nlatham1999/GoApp](https://github.com/nlatham1999/GoApp)。请随时评论你的想法和问题！

如果您想要更多资源，我在下面提供了几个链接:

去和 MongoDB:
[https://www . MongoDB . com/blog/post/quick-start-golang-MongoDB-starting-and-setup](https://www.mongodb.com/blog/post/quick-start-golang-mongodb-starting-and-setup)
[https://www.mongodb.com/blog/post/quick-start-golang-MongoDB-how-to-create-documents](https://www.mongodb.com/blog/post/quick-start-golang--mongodb--how-to-create-documents)
[https://www.mongodb.com/blog/post/quick-start-golang-MongoDB-how-to-read-documents](https://www.mongodb.com/blog/post/quick-start-golang--mongodb--how-to-read-documents)
[https://www.mongodb.com/blog/post/quick-start-golang-MongoDB-how-to-update-documents](https://www.mongodb.com/blog/post/quick-start-golang--mongodb--how-to-update-documents)
[https://www.mongodb.com/blog/post/quick-start-golang-MongoDB-how-to-delete-documents](https://www.mongodb.com/blog/post/quick-start-golang--mongodb--how-to-delete-documents)

反应过来:
[https://react-bootstrap.github.io/components/alerts](https://react-bootstrap.github.io/components/alerts)
[https://www.w3schools.com/react/](https://www.w3schools.com/react/)