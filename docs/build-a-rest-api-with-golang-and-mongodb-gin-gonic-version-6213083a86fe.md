# 用 Golang 和 MongoDB 构建一个 REST API—Gin-gonic 版本

> 原文：<https://medium.com/geekculture/build-a-rest-api-with-golang-and-mongodb-gin-gonic-version-6213083a86fe?source=collection_archive---------3----------------------->

![](img/b632f389be8b82fab99eba2373513785.png)

cover photo

表述性状态转移(REST)是一种指导应用程序编程接口(API)设计和开发的架构模式。REST APIs 已经成为产品的服务器部分和它的客户机之间的通信标准，以提高性能、可伸缩性、简单性、可修改性、可见性、可移植性和可靠性。

这篇文章将讨论使用 [Gin-gonic](https://github.com/gin-gonic/gin) 框架和 [MongoDB](https://www.mongodb.com/) 用 Golang 构建一个用户管理应用。在本教程的最后，我们将学习如何构建一个 Gin-gonic 应用程序，构建一个 REST API 并使用 MongoDB 持久化我们的数据。

Gin-gonic，俗称 **Gin** ，是一个用 Golang 编写的 HTTP web 框架，具有性能和生产力支持。Gin 使用定制版本的 [HttpRouter](https://github.com/julienschmidt/httprouter) ，这是一个轻量级、高性能的 HTTP 请求路由器，它在 API 路由中的导航速度比大多数框架都要快。

MongoDB 是一个基于文档的数据库管理程序，用作关系数据库的替代方案。MongoDB 支持处理大型分布式数据集，并提供无缝存储或检索信息的选项。

您可以在这个[库](https://github.com/Mr-Malomz/gin-mongo-api)中找到完整的源代码。

# 先决条件

这篇文章中的以下步骤需要 Golang 经验。使用 MongoDB 的经验不是必需的，但是拥有它是很好的。

我们还需要以下物品:

*   一个 [MongoDB 帐户](https://www.mongodb.com/)来托管数据库。 [**报名**](https://www.mongodb.com/cloud/atlas/register) **完全免费**。
*   [Postman](https://www.postman.com/downloads/) 或者你选择的任何 API 测试应用

# 让我们编码

## 入门指南

首先，我们需要导航到所需的目录，并在我们的终端中运行下面的命令

```
mkdir gin-mongo-api && cd gin-mongo-api
```

该命令创建一个`gin-mongo-api`文件夹，并导航到项目目录。

接下来，我们需要通过运行以下命令来初始化 Go 模块以管理项目依赖关系:

```
go mod init gin-mongo-api
```

该命令将创建一个`go.mod`文件，用于跟踪项目依赖关系。

我们继续安装所需的依赖项:

```
go get -u github.com/gin-gonic/gin go.mongodb.org/mongo-driver/mongo github.com/joho/godotenv github.com/go-playground/validator/v10
```

`github.com/gin-gonic/gin`是一个用于构建 web 应用程序的框架。

`go.mongodb.org/mongo-driver/mongo`是连接 MongoDB 的驱动。

`github.com/joho/godotenv`是一个管理环境变量的库。

`github.com/go-playground/validator/v10`是一个用于验证结构和字段的库。

# 应用程序入口点

安装好项目依赖项后，我们需要在根目录下创建`main.go`文件，并添加下面的代码片段:

上面的代码片段执行了以下操作:

*   导入所需的依赖项。
*   使用`Default`配置初始化 Gin 路由器。默认功能用默认中间件(记录器和恢复)配置 Gin 路由器。
*   使用`Get`函数路由到`/`路径和返回`JSON of Hello from Gin-gonic & mongoDB`的处理函数。
*   使用`Run`功能将路由器连接到 **http。服务器**并开始监听和服务`localhost:6000`上的 HTTP 请求。

接下来，我们可以通过在终端中运行下面的命令来启动开发服务器，从而测试我们的应用程序。

```
go run main.go
```

![](img/be0651162d0af9dc897e3d3eff7379e4.png)

# Golang 的模块化

对于我们的项目来说，有一个好的文件夹结构是很重要的。良好的项目结构简化了我们在应用程序中处理依赖关系的方式，并使我们和其他人更容易阅读我们的代码库。

为此，我们需要在我们的项目目录中创建`configs`、`controllers`、`models`、`responses`和`routes`文件夹。

![](img/3d5e16a7b5ddf0fd975603f61fb946e6.png)

**PS**:*`*go.sum*`*文件包含所有的依赖校验和，由* `*go*` *工具管理。我们不必为此担心。**

*`configs`用于模块化项目配置文件*

*控制器用于模块化应用逻辑。*

*`models`用于模块化数据和数据库逻辑。*

*`responses`用于模块化文件，描述我们希望 API 给出的响应。这一点以后会变得更加清楚。*

*`routes`用于模块化 URL 模式和处理程序信息。*

# *设置 MongoDB*

*完成后，我们需要登录或注册我们的 [MongoDB](https://www.mongodb.com/) 账户。点击项目下拉菜单并点击**新项目**按钮。*

*![](img/17b94dbdc2f672e5b64f512165a71006.png)*

*输入`golang-api`作为项目名称，点击下一个**，点击**创建项目。*****

*![](img/af63e074ce5eb7595605845c750d1131.png)**![](img/763a7d2748cc5deac431382d966208d7.png)*

*点击**建立数据库***

*![](img/e12dc6f1bc6acc8f8e6c40a40e890071.png)*

*选择**共享**作为数据库类型。*

*![](img/109131f76f9b96db04c6d13135c87d4c.png)*

*点击**创建**以设置集群。这可能需要一些时间来设置。*

*![](img/6393f7c63c3b45030c9a162719d2a9a5.png)*

*接下来，我们需要通过输入**用户名**、**密码**然后点击**创建用户**来创建一个从外部访问数据库的用户。我们还需要添加我们的 IP 地址，以便通过点击**添加我当前的 IP 地址**按钮安全地连接到数据库。然后点击**完成并关闭**保存更改。*

*![](img/418c9b337955e72701bdaf08534ecb2f.png)**![](img/90778640ec4a902e3778010334626a9d.png)*

*保存更改后，我们应该会看到一个数据库部署屏幕，如下所示:*

*![](img/5f892cff5a3065b3bc4968c98c6e0faf.png)*

# *将我们的应用程序连接到 MongoDB*

*配置完成后，我们需要将应用程序与创建的数据库连接起来。为此，点击**连接**按钮*

*![](img/57b574cbbaf742ffc2156ca6fd3d6f7e.png)*

*点击**连接你的应用**，更改**驱动**即可，而**版本**如下图所示。然后点击复制图标**复制连接字符串。***

*![](img/485bb14bd084d73ced8bf1820ec6a53c.png)**![](img/7c3e825dedfb235d45d2967513d0a0d4.png)*

***设置环境变量** 接下来，我们必须用我们之前创建的用户密码修改复制的连接字符串，并更改数据库名称。为此，首先，我们需要在根目录中创建一个`.env`文件，并在该文件中添加下面的代码片段:*

```
*MONGOURI=mongodb+srv://<YOUR USERNAME HERE>:<YOUR PASSWORD HERE>@cluster0.e5akf.mongodb.net/myFirstDatabese?retryWrites=true&w=majority*
```

*下面是正确填充的连接字符串示例:*

```
*MONGOURI=mongodb+srv://malomz:malomzPassword@cluster0.e5akf.mongodb.net/golangDB?retryWrites=true&w=majority*
```

*![](img/27d2201f6f03e6f52c242f9e4b9924d4.png)*

***加载环境变量** 完成后，我们需要创建一个助手函数来使用我们之前安装的`github.com/joho/godotenv`库加载环境变量。为此，我们需要导航到`configs`文件夹，在这个文件夹中，创建一个`env.go`文件，并添加下面的代码片段:*

*上面的代码片段执行了以下操作:*

*   *导入所需的依赖项。*
*   *创建一个`EnvMongoURI`函数，检查环境变量是否被正确加载并返回环境变量。*

***连接到 MongoDB** 要从我们的应用程序连接到 MongoDB 数据库，首先我们需要导航到`configs`文件夹，在这个文件夹中，创建一个`setup.go`文件并添加下面的代码片段:*

*上面的代码片段执行了以下操作:*

*   *导入所需的依赖项。*
*   *创建一个`ConnectDB`函数，首先配置客户机使用正确的 URI 并检查错误。其次，我们定义了一个 10 秒的超时，我们希望在尝试连接时使用。第三，检查连接到数据库时是否有错误，如果连接时间超过 10 秒，则取消连接。最后，我们 pinged 数据库来测试我们的连接，并返回客户端实例。*
*   *创建一个`ConnectDB`的`DB`变量实例。这将在创建收藏时派上用场。*
*   *创建一个`GetCollection`函数来检索并在数据库上创建`collections`。*

*接下来，我们需要在应用程序启动时连接到数据库。为此，我们需要修改`main.go`，如下所示:*

# *设置 API 路由处理程序和回应类型*

***Route Handler** 完成后，我们需要在`routes`文件夹中创建一个`user_route.go`文件来管理应用程序中所有与用户相关的路线，如下所示:*

*接下来，我们需要将新创建的路由附加到 **http。通过修改`main.go`中的服务器**，如下所示:*

*接下来，我们需要创建一个可重用的结构来描述我们的 API 的响应。为此，导航到`responses`文件夹，在该文件夹中，创建一个`user_response.go`文件并添加以下代码片段:*

*上面的代码片段创建了一个具有`Status`、`Message`和`Data`属性的`UserResponse`结构来表示 API 响应类型。*

***PS** : `*json:”status”*` *，* `*json:”message”*` *，* `*json:”data”*` *称为* ***struct 标签*** *。结构标签允许我们将元信息附加到相应的结构属性上。换句话说，我们使用它们来重新格式化 API 返回的 JSON 响应。**

# *最后，创建 REST API*

*接下来，我们需要一个模型来表示我们的应用程序数据。为此，我们需要导航到`models`文件夹，在该文件夹中，创建一个`user_model.go`文件并添加下面的代码片段:*

*上面的代码片段执行了以下操作:*

*   *导入所需的依赖项。*
*   *创建一个具有所需属性的`User`结构。我们将`omitempty`和`validate:”required”`添加到 struct 标签中，分别告诉 Gin-gonic 忽略空字段并使字段成为必填字段。*

***创建用户端点** 有了模型设置，我们现在可以创建一个函数来创建用户。为此，我们需要导航到`controllers`文件夹，在该文件夹中，创建一个`user_controller.go`文件并添加下面的代码片段:*

*上面的代码片段执行了以下操作:*

*   *导入所需的依赖项。*
*   *创建`userCollection`和`validate`变量，分别使用我们之前安装的`github.com/go-playground/validator/v10`库来创建集合和验证模型。*
*   *创建一个返回 Gin-gonic 处理程序的`CreateUser`函数。在返回的处理程序中，当将用户插入到文档中时，我们首先定义了一个 10 秒的超时，使用验证器库来验证请求体和必填字段。我们使用之前创建的`UserResponse`结构返回了适当的消息和状态代码。其次，我们创建了一个`newUser`变量，使用`userCollection.InsertOne`函数插入它，并检查是否有错误。最后，如果插入成功，我们返回正确的响应。*

*接下来，我们需要用路由 API URL 和相应的控制器更新`user_routes.go`,如下所示:*

***获取一个用户端点** 要获取一个用户的详细信息，我们需要修改`user_controller.go`如下所示:*

*上面的代码片段执行了以下操作:*

*   *导入所需的依赖项。*
*   *创建一个返回 Gin-gonic 处理程序的`GetAUser`函数。在返回的处理程序中，我们首先定义了在文档中查找用户时 10 秒的超时，一个从 URL 参数中获取用户 Id 的 userId 变量和一个用户变量。我们将`userId`从字符串转换为`primitive.ObjectID`类型，即 MongoDB 使用的 **BSON** 类型。其次，我们使用`userCollection.FindOne`搜索用户，将`objId`作为过滤器传递，并使用`Decode`属性方法获取相应的对象。最后，我们返回解码的响应。*

*接下来，我们需要用 route API URL 和相应的控制器更新`user_routes.go`,如下所示:*

***PS:** *我们还传递了一个* `*userId*` *作为 URL 路径的参数。指定的参数必须与我们在控制器中指定的参数相匹配。**

***编辑用户端点***

*要编辑用户，我们需要修改`user_controller.go`，如下所示:*

*上面的`EditAUser`功能与`CreateUser`功能的作用相同。然而，我们包含了一个`update`变量来获取更新的字段，并使用`userCollection.UpdateOne`更新了集合。最后，我们搜索更新后的用户详细信息，并返回解码后的响应。*

*接下来，我们需要用路由 API URL 和相应的控制器更新`user_routes.go`,如下所示:*

***删除用户端点** 要删除用户，我们需要修改`user_controller.go`如下所示:*

*`DeleteAUser`功能遵循前面的步骤，使用`userCollection.DeleteOne`删除匹配的记录。我们还检查一个项目是否被成功删除，并返回适当的响应。*

*接下来，我们需要用路由 API URL 和相应的控制器更新`user_routes.go`,如下所示:*

***获取用户列表端点** 要获取用户列表，我们需要修改`user_controller.go`如下所示:*

*`GetAllUsers`函数遵循前面的步骤，使用`userCollection.Find`获取用户列表。我们还使用`Next`属性方法遍历返回的用户列表，以最佳方式读取返回的列表。*

*接下来，我们需要用路由 API URL 和相应的控制器更新 user_routes.go，如下所示:*

***完成 user_controller.go***

***完成 user_route.go***

*完成后，我们可以通过在终端中运行下面的命令启动开发服务器来测试我们的应用程序。*

```
*go run main.go*
```

*![](img/e2c850b46038c89712df036922be9e3d.png)**![](img/45410a2bc5a837d5bd899357690e4674.png)**![](img/088115ed9beba14d2cce9ec9bea54a52.png)**![](img/c8404cd1accec3ee8fb5e6096e97cf05.png)**![](img/74fcb9711fc7daf9834b7d9d90a289cd.png)**![](img/ca664a0c0fa96c1666368d4b064f9fde.png)**![](img/e3e956ab79a25b7ae36c3ea12b668d1d.png)*

# *结论*

*这篇文章讨论了如何构建 Gin-gonic 应用程序，构建 REST API，以及使用 MongoDB 持久化我们的数据。*

*您可能会发现这些资源很有帮助:*

*   *[Gin-gonic](https://github.com/gin-gonic/gin)*
*   *[MongoDB Go 驱动](https://docs.mongodb.com/drivers/go/current/)*
*   *[Go 验证器](https://github.com/go-playground/validator)*
*   *[转到环境加载器](https://github.com/joho/godotenv)*