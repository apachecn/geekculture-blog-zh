# 用 Golang 和 MongoDB 构建一个 REST API—Echo 版本

> 原文：<https://medium.com/geekculture/build-a-rest-api-with-golang-and-mongodb-echo-version-be48eccdefe3?source=collection_archive---------4----------------------->

![](img/390ba8ccd219608d565fda53d23cde89.png)

cover photo

表述性状态转移(REST)是一种指导应用程序编程接口(API)设计和开发的架构模式。REST APIs 已经成为产品的服务器部分和它的客户机之间的通信标准，以提高性能、可伸缩性、简单性、可修改性、可见性、可移植性和可靠性。

这篇文章将讨论用 Golang 使用 [Echo](https://github.com/labstack/echo) 框架和 [MongoDB](https://www.mongodb.com/) 构建一个用户管理应用程序。在本教程的最后，我们将学习如何构建一个 Echo 应用程序，构建一个 REST API 并使用 MongoDB 持久化我们的数据。

Echo 是一个基于 Golang 的 HTTP web 框架，具有高性能和可扩展性。它支持优化的路由、中间件、模板、数据绑定和呈现。

MongoDB 是一个基于文档的数据库管理程序，用作关系数据库的替代方案。MongoDB 支持处理大型分布式数据集，并提供无缝存储或检索信息的选项。

您可以在这个[资源库](https://github.com/Mr-Malomz/echo-mongo-api)中找到完整的源代码。

# 先决条件

这篇文章中的以下步骤需要 Golang 经验。使用 MongoDB 的经验不是必需的，但是拥有它是很好的。

我们还需要以下物品:

*   一个 [MongoDB 帐户](https://www.mongodb.com/)来托管数据库。 [**报名**](https://www.mongodb.com/cloud/atlas/register) **完全免费**。
*   [Postman](https://www.postman.com/downloads/) 或您选择的任何 API 测试应用程序

# 让我们编码

## 入门指南

首先，我们需要导航到所需的目录，并在我们的终端中运行下面的命令

```
mkdir echo-mongo-api && cd echo-mongo-api
```

该命令创建一个`echo-mongo-api`文件夹，并导航到项目目录。

接下来，我们需要通过运行以下命令来初始化 Go 模块以管理项目依赖关系:

```
go mod init echo-mongo-api
```

该命令将创建一个`go.mod`文件，用于跟踪项目依赖关系。

我们继续安装所需的依赖项:

```
go get -u github.com/labstack/echo/v4 go.mongodb.org/mongo-driver/mongo github.com/joho/godotenv github.com/go-playground/validator/v10
```

`github.com/labstack/echo/v4`是一个用于构建 web 应用程序的框架。

`go.mongodb.org/mongo-driver/mongo`是连接 MongoDB 的驱动。

`github.com/joho/godotenv`是一个管理环境变量的库。

`github.com/go-playground/validator/v10`是一个用于验证结构和字段的库。

# 应用程序入口点

安装了项目依赖项后，我们需要在根目录下创建`main.go`文件，并添加下面的代码片段:

上面的代码片段执行了以下操作:

*   导入所需的依赖项。
*   使用`New`函数初始化一个 Echo 应用程序。
*   使用`Get`函数路由到`/`路径和一个返回`Hello from Echo & mongoDB`的 JSON 的处理函数。`echo.Map`是`map[string]interface{}`的快捷方式，对 JSON 返回有用。
*   使用`Start`功能运行端口`6000`上的应用程序。

接下来，我们可以通过在终端中运行下面的命令来启动开发服务器，从而测试我们的应用程序。

```
go run main.go
```

![](img/7afc251cc5d9ebe9e72d11b1c6d7cbf7.png)

# Golang 的模块化

对于我们的项目来说，有一个好的文件夹结构是很重要的。良好的项目结构简化了我们在应用程序中处理依赖关系的方式，并使我们和其他人更容易阅读我们的代码库。

为此，我们需要在我们的项目目录中创建`configs`、`controllers`、`models`、`responses`和`routes`文件夹。

![](img/756fa08763f8bde49bdca815e5b853a1.png)

**PS**:*`*go.sum*`*文件包含所有依赖校验和，由 go 工具管理。我们不必为此担心。**

*`configs`用于模块化项目配置文件*

*`controllers`用于模块化应用逻辑。*

*`models`用于模块化数据和数据库逻辑。*

*`responses`用于模块化文件，描述我们希望 API 给出的响应。这一点以后会变得更加清楚。*

*`routes`用于模块化 URL 模式和处理程序信息。*

# *设置 MongoDB*

*完成后，我们需要登录或注册我们的 [MongoDB](https://www.mongodb.com/) 账户。点击项目下拉菜单并点击**新建项目**按钮。*

*![](img/54c282259b81a64db3785587f077ca04.png)*

*输入`golang-api`作为项目名称，点击**下一个**，点击**创建项目。***

*![](img/bc74cf242d957422dfb47b9e9428db73.png)**![](img/21d3ef0257f4c2afd7087c7b9e6a95a7.png)*

*点击**建立数据库***

*![](img/0f01339502e1d1ead06de9c54d793e3b.png)*

*选择**共享**作为数据库类型。*

*![](img/86058f2bc2c7bfbbe1b3d41c607da3e8.png)*

*点击**创建**来设置集群。这可能需要一些时间来设置。*

*![](img/3a2e64b7fc28659862b99af1b8496a34.png)*

*接下来，我们需要通过输入**用户名**、**密码**然后点击**创建用户**来创建一个从外部访问数据库的用户。我们还需要添加我们的 IP 地址，以便通过点击**添加我当前的 IP 地址**按钮安全地连接到数据库。然后点击**完成并关闭**保存更改。*

*![](img/73e0230f90695ae5414765195fa11396.png)**![](img/30940bdc25f621de9d722e20c50058e8.png)*

*保存更改后，我们应该会看到一个数据库部署屏幕，如下所示:*

*![](img/d59cdae73564202e0c5229c8eb5ce2cc.png)*

# *将我们的应用程序连接到 MongoDB*

*配置完成后，我们需要将应用程序与创建的数据库连接起来。为此，点击**连接**按钮*

*![](img/ef3d81f6b5cc3b0f83da24cb7a6e9e14.png)*

*点击**连接你的应用**，将**驱动**改为`Go`，并且**版本**如下图所示。然后点击复制**图标**复制连接字符串。*

*![](img/5e1df0f1517652d5ed8205f7b5621d44.png)**![](img/61183ca632aa58289219d6fe3ded6b41.png)*

***设置环境变量***

*接下来，我们必须用之前创建的用户密码修改复制的连接字符串，并更改数据库名称。为此，首先，我们需要在根目录中创建一个`.env`文件，并在该文件中添加下面的代码片段:*

```
*MONGOURI=mongodb+srv://<YOUR USERNAME HERE>:<YOUR PASSWORD HERE>@cluster0.e5akf.mongodb.net/myFirstDatabese?retryWrites=true&w=majority*
```

*下面是正确填充的连接字符串示例:*

```
*MONGOURI=mongodb+srv://malomz:malomzPassword@cluster0.e5akf.mongodb.net/golangDB?retryWrites=true&w=majority*
```

*![](img/5d59cb235315ca1da2403a905f500860.png)*

***加载环境变量***

*完成后，我们需要创建一个助手函数，使用我们之前安装的`github.com/joho/godotenv`库来加载环境变量。为此，我们需要导航到`configs`文件夹，在这个文件夹中，创建一个`env.go`文件，并添加下面的代码片段:*

*上面的代码片段执行了以下操作:*

*   *导入所需的依赖项。*
*   *创建一个`EnvMongoURI`函数，检查环境变量是否被正确加载并返回环境变量。*

***连接到 MongoDB** 要从我们的应用程序连接到 MongoDB 数据库，首先，我们需要导航到`configs`文件夹，在这个文件夹中，创建一个`setup.go`文件并添加下面的代码片段:*

*上面的代码片段执行了以下操作:*

*   *导入所需的依赖项。*
*   *创建一个`ConnectDB`函数，首先配置客户端使用正确的 URI 并检查错误。其次，我们定义了一个 10 秒的超时，我们希望在尝试连接时使用。第三，检查连接到数据库时是否有错误，如果连接时间超过 10 秒，则取消连接。最后，我们 pinged 数据库来测试我们的连接，并返回客户端实例。*
*   *创建一个`ConnectDB`的`DB`变量实例。这将在创建收藏时派上用场。*
*   *创建一个`GetCollection`函数来检索并在数据库上创建`collections`。*

*接下来，我们需要在应用程序启动时连接到数据库。为此，我们需要修改`main.go`，如下所示:*

# *设置 API 路由处理程序和回应类型*

***路线处理程序** 完成后，我们需要在`routes`文件夹中创建一个`user_route.go`文件来管理我们应用程序中所有与用户相关的路线，如下所示:*

*接下来，我们需要将新创建的路由附加到 **http。通过修改服务器`main.go`中的**，如下所示:*

***响应类型***

*接下来，我们需要创建一个可重用的结构来描述我们的 API 的响应。为此，导航到`responses`文件夹，在该文件夹中，创建一个`user_response.go`文件并添加下面的代码片段:*

*上面的代码片段创建了一个具有`Status`、`Message`和`Data`属性的`UserResponse`结构来表示 API 响应类型。*

***PS** : `*json:”status”*` *，* `*json:”message”*` *，* `*json:”data”*` *称为* ***struct 标签*** *。结构标签允许我们将元信息附加到相应的结构属性上。换句话说，我们使用它们来重新格式化 API 返回的 JSON 响应。**

# *最后，创建 REST API*

*接下来，我们需要一个模型来表示我们的应用程序数据。为此，我们需要导航到`models`文件夹，在该文件夹中，创建一个`user_model.go`文件并添加下面的代码片段:*

*上面的代码片段执行了以下操作:*

*   *导入所需的依赖项。*
*   *创建一个具有所需属性的`User`结构。我们在 struct 标签中添加了`omitempty`和`validate:”required”`,分别告诉纤程忽略空字段并使字段成为必填字段。*

***创建用户端点** 有了模型设置，我们现在可以创建一个函数来创建用户。为此，我们需要导航到`controllers`文件夹，在这个文件夹中，创建一个`user_controller.go`文件并添加下面的代码片段:*

*上面的代码片段执行了以下操作:*

*   *导入所需的依赖项。*
*   *创建`userCollection`和`validate`变量，分别使用我们之前安装的`github.com/go-playground/validator/v10`库来创建集合和验证模型。*
*   *创建一个返回错误的`CreateUser`函数。在函数内部，我们首先定义了一个 10 秒的超时时间，用于在文档中插入用户，使用验证器库验证请求体和必填字段。我们使用之前创建的`UserResponse`结构返回了适当的消息和状态代码。其次，我们创建了一个`newUser`变量，使用`userCollection.InsertOne`函数插入它，并检查是否有错误。最后，如果插入成功，我们返回正确的响应。*

*接下来，我们需要用路由 API URL 和相应的控制器更新`user_routes.go`,如下所示:*

***获取一个用户端点** 要获取一个用户的详细信息，我们需要修改`user_controller.go`如下所示:*

*上面的代码片段执行了以下操作:*

*   *导入所需的依赖项。*
*   *创建一个返回错误的`GetAUser`函数。在函数内部，我们首先定义了在文档中查找用户时 10 秒的超时，一个从 URL 参数中获取用户 Id 的 userId 变量和一个用户变量。我们将`userId`从字符串转换为`primitive.ObjectID`类型，这是 MongoDB 使用的 **BSON** 类型。其次，我们使用`userCollection.FindOne`搜索用户，将`objId`作为过滤器传递，并使用`Decode`属性方法获取相应的对象。最后，我们返回解码的响应。*

*接下来，我们需要用路由 API URL 和相应的控制器更新`user_routes.go`,如下所示:*

***PS:** *我们还传递了一个* `*userId*` *作为 URL 路径的参数。指定的参数必须与我们在控制器中指定的参数相匹配。**

***编辑用户端点** 要编辑用户，我们需要修改`user_controller.go`如下所示:*

*上面的`EditAUser`函数与 CreateUser 函数做同样的事情。然而，我们包含了一个`update`变量来获取更新的字段，并使用`userCollection.UpdateOne`更新了集合。最后，我们搜索更新后的用户详细信息，并返回解码后的响应。*

*接下来，我们需要用 route API URL 和相应的控制器更新`user_routes.go`,如下所示:*

***删除一个用户端点***

*要删除一个用户，我们需要修改`user_controller.go`，如下所示:*

*`DeleteAUser`功能遵循前面的步骤，使用`userCollection.DeleteOne`删除匹配的记录。我们还检查一个项目是否被成功删除，并返回适当的响应。*

*接下来，我们需要用路由 API URL 和相应的控制器更新`user_routes.go`，如下所示:*

***获取用户列表端点** 要获取用户列表，我们需要修改`user_controller.go`如下所示:*

*`GetAllUsers`功能遵循前面的步骤，使用`userCollection.Find`获取用户列表。我们还使用`Next`属性方法遍历返回的用户列表，以最佳方式读取返回的列表。*

*接下来，我们需要用路由 API URL 和相应的控制器更新`user_routes.go`,如下所示:*

***完成用户 _ 控制器. go***

***完成 user_route.go***

*完成后，我们可以通过在终端中运行下面的命令启动开发服务器来测试我们的应用程序。*

```
*go run main.go*
```

*![](img/5bd8698724628ca852f470b105c6c0d5.png)**![](img/1698892dec5f22cb214b2d34bddba309.png)**![](img/753c4e7e3a9405e169237b37ab377dad.png)**![](img/9feddaf5eaaf70336a40abb4b3163f7e.png)**![](img/ba7b99d54a11f861cccf3b41bbffc6a3.png)**![](img/4c8f54af377466bbd1b29de60ebf943c.png)**![](img/8e57c53e411e1f8ed0db14aeb14e5a63.png)*

# *结论*

*这篇文章讨论了如何构建一个 Echo 应用程序，构建一个 REST API，并使用 MongoDB 持久化我们的数据。*

*您可能会发现这些资源很有帮助:*

*   *[回音](https://github.com/labstack/echo)*
*   *[MongoDB Go 驱动](https://docs.mongodb.com/drivers/go/current/)*
*   *[Go 验证器](https://github.com/go-playground/validator)*
*   *[Go 环境加载器](https://github.com/joho/godotenv)*