# 在 Golang 和 MongoDB 中创建一个基于 GraphQL 的项目管理端点

> 原文：<https://medium.com/geekculture/create-a-graphql-powered-project-management-endpoint-in-golang-and-mongodb-98a7a8ca483f?source=collection_archive---------7----------------------->

![](img/aca5d479a8dec3e5c789620130b16d0e.png)

cover

GraphQL 是一种用于读取和操作 API 数据的查询语言。它通过提供灵活、直观的语法来描述数据，从而优先向客户端或服务器提供准确的数据需求。

与传统的 REST API 相比，GraphQL 提供了一个类型系统来描述数据的模式，从而为 API 的消费者提供了使用单个端点来探索和请求所需数据的能力。

这篇文章将讨论用 Golang 使用 [gqlgen](https://github.com/99designs/gqlgen) 库和 [MongoDB](https://www.mongodb.com/) 构建一个项目管理应用程序。在本教程的最后，我们将学习如何创建一个支持读取和操作项目管理数据的 GraphQL 端点，并使用 MongoDB 持久化我们的数据。

GitHub 资源库可以在[这里](https://github.com/Mr-Malomz/project-mngt-golang-graphql-gin)找到。

# 先决条件

要完全掌握本教程中介绍的概念，需要 Golang 方面的经验。使用 MongoDB 的经验不是必需的，但是拥有它是很好的。

我们还需要以下物品:

*   GraphQL 的基础知识
*   一个 [MongoDB 帐户](https://www.mongodb.com/)来托管数据库。 [**报名**](https://www.mongodb.com/cloud/atlas/register) **完全免费**

# 让我们编码

## 入门指南

首先，我们需要导航到所需的目录，并在我们的终端中运行下面的命令

```
mkdir project-mngt-golang-graphql && cd project-mngt-golang-graphql
```

该命令创建一个`project-mngt-golang-graphql`文件夹，并导航到项目目录。

接下来，我们需要通过运行以下命令来初始化 Go 模块以管理项目依赖关系:

```
go mod init project-mngt-golang-graphql
```

该命令将创建一个`go.mod`文件，用于跟踪项目依赖关系。

我们继续安装所需的依赖项:

```
go get github.com/99designs/gqlgen go.mongodb.org/mongo-driver/mongo github.com/joho/godotenv
```

`github.com/99designs/gqlgen`是一个用于在 Go 中创建 GraphQL 应用程序的库。

`go.mongodb.org/mongo-driver/mongo`是连接 MongoDB 的驱动。

`github.com/joho/godotenv`是一个管理环境变量的库。

# 项目初始化

gqlgen 库使用模式优先的方法；它允许我们使用 GraphQL 的模式定义语言来定义我们的 API。该库还让我们通过生成项目样板文件来关注实现。

要生成项目样板文件，我们需要运行下面的命令:

```
go run github.com/99designs/gqlgen init
```

上面的命令会生成以下文件:

*   `gqlgen.yml`用于配置 gqlgen 的文件
*   `graph/generated/generated.go`包含 gqlgen 在执行过程中自动生成的所有代码的文件。我们不需要编辑这个文件。
*   `graph/model/models_gen.go`包含构建 GraphQL 所需的生成模型的文件。这个文件也是由 gqlgen 自动生成的。
*   `graph/schema.graphqls`定义我们的模式的文件。
*   `graph/schema.resolvers.go`一个定义我们应用程序逻辑的文件。
*   `server.go`这个文件是我们的应用入口点。

PS: *我们可能会得到一个关于缺少依赖项的错误。我们可以通过重新安装之前安装的软件包来解决这个问题。*

```
go get github.com/99designs/gqlgen go.mongodb.org/mongo-driver/mongo github.com/joho/godotenv
```

# 设置 MongoDB

完成后，我们需要登录或注册我们的 [MongoDB](/) 账户。点击项目下拉菜单并点击**新建项目**按钮。

![](img/a8c3496f2b5500b37e2b93d531267917.png)

输入`projectMngt`作为项目名称，点击下一个**，点击**创建项目。****

![](img/3d39804183e12bbc0838b8e6328db97b.png)![](img/ef8ea4aa1c83a941474a466d516ff415.png)

点击**建立数据库**

![](img/8078ee686c757414a548d0b7a1540b6f.png)

选择**共享**作为数据库类型。

![](img/d88c344ec8de44caa130f9cbd8f81f44.png)

点击**创建**来设置集群。这可能需要一些时间来设置。

![](img/a6f2be1e72661d7cd0387cd04348cb84.png)

接下来，我们需要创建一个用户，通过输入**用户名**、**密码**，然后点击**创建用户**，从外部访问数据库。我们还需要添加我们的 IP 地址，以便通过点击**添加我当前的 IP 地址**按钮安全地连接到数据库。然后点击**完成并关闭**保存更改。

![](img/8f1d3dc1b2b02aae780240cd0d9cd419.png)![](img/d1fefc6346c4400e3f4228d903f03702.png)

保存更改后，我们应该会看到一个数据库部署屏幕，如下所示:

![](img/fd08eb4d9bc779bbc90f70288a81f643.png)

# 将我们的应用程序连接到 MongoDB

配置完成后，我们需要将应用程序与创建的数据库连接起来。为此，点击**连接**按钮

![](img/ba3f7ff97d3c734a771ae6ac7f755ab9.png)

点击**连接你的应用**，改变**驱动**为`Go`和**版本**，如下图所示。然后点击复制**图标**复制连接字符串。

![](img/430d530d0c6ac771612699bc007723e7.png)![](img/785a5d094c2cc1ea792bf32c6adf0ffd.png)

**设置环境变量** 接下来，我们必须用我们之前创建的用户密码修改复制的连接字符串，并更改数据库名称。为此，首先，我们需要在根目录中创建一个`.env`文件，并在该文件中添加以下代码片段:

```
MONGOURI=mongodb+srv://<YOUR USERNAME HERE>:<YOUR PASSWORD HERE>@cluster0.e5akf.mongodb.net/<DATABASE NAME>?retryWrites=true&w=majority
```

下面是正确填充的连接字符串示例:

```
MONGOURI=mongodb+srv://malomz:malomzPassword@cluster0.e5ahghkf.mongodb.net/projectMngt?retryWrites=true&w=majority
```

**加载环境变量** 完成后，我们需要创建一个助手函数，使用我们之前安装的`github.com/joho/godotenv`库来加载环境变量。为此，我们需要在根目录下创建一个`configs`文件夹；在这里，创建一个`env.go`文件并添加下面的代码片段:

上面的代码片段执行了以下操作:

*   导入所需的依赖项。
*   创建一个`EnvMongoURI`函数，检查环境变量是否被正确加载并返回环境变量。

# 定义我们的模式

为此，我们需要导航到`graph`文件夹，并在该文件夹中更新`schema.graphqls`文件，如下所示:

上面的代码片段通过创建两种类型定义了我们的 API 所需的模式；一个`Project`和一个`Owner`。我们还定义了`Query`来对类型执行操作，`inputs`来定义创建属性，`Mutation`来创建项目和所有者。

# 创建应用程序逻辑

接下来，我们需要使用`gqlgen`库为我们新创建的模式生成逻辑。为此，我们需要在终端中运行以下命令:

```
go run github.com/99designs/gqlgen generate
```

在运行上面的命令时，我们将得到关于`schema.resolvers.go`文件中缺少`Todo`模型的错误；这是因为我们更改了默认模型。我们可以通过删除`CreateTodo`和`Todo`功能来修复错误。删除后，我们的代码应该类似于下面的代码片段:

**创建数据库逻辑** 随着 GraphQL 逻辑的生成，我们需要创建代码对应的数据库逻辑。为此，我们需要导航到`configs`文件夹，在这里，创建一个`db.go`文件并添加下面的代码片段:

上面的代码片段执行了以下操作:

*   导入所需的依赖项
*   创建一个带有客户端字段的`DB`结构来访问 MongoDB。
*   创建一个`ConnectDB`函数，首先配置`client`使用正确的 URI 并检查错误。其次，我们定义了一个 10 秒的超时，我们希望在尝试连接时使用。第三，检查连接到数据库时是否有错误，如果连接时间超过 10 秒，则取消连接。最后，我们 pinged 数据库来测试我们的连接，并返回一个指向`DB`结构的指针。
*   创建一个`colHelper`函数来创建一个集合。
*   创建一个将`DB`结构作为指针接收器的`CreateProject`函数，并返回创建的`Project`或`Error`。在函数内部，我们还创建了一个项目集合，在向集合中插入数据时定义了 10 秒的超时，并使用`InsertOne`函数来插入输入。
*   创建一个将`DB`结构作为指针接收器的`CreateOwner`函数，并返回创建的`Owner`或`Error`。在函数内部，我们还创建了一个所有者集合，定义了在向集合中插入数据时 10 秒的超时，并使用了`InsertOne`函数来插入输入。
*   创建一个将`DB`结构作为指针接收器的`GetOwners`函数，并返回`Owners`或`Error`的列表。该函数遵循前面的步骤，使用`Find`函数获取所有者列表。我们还使用`Next`属性方法遍历返回的所有者列表，以最佳方式读取返回的列表。
*   创建一个将`DB`结构作为指针接收器的`GetProjects`函数，并返回`Projects`或`Error`的列表。该函数遵循前面的步骤，使用`Find`函数获取项目列表。我们还使用`Next`属性方法遍历返回的项目列表，以最佳方式读取返回的列表。
*   创建一个将`DB`结构作为指针接收器的`SingleOwner`函数，并使用`FindOne`函数返回匹配的`Owner`或错误。
*   创建一个`SingleProject`函数，该函数将 DB struct 作为指针接收器，并使用 FindOne 函数返回匹配的项目或错误。

**更新应用程序逻辑** 接下来，我们需要用数据库函数更新应用程序逻辑。为此，我们需要更新`schema.resolvers.go`文件，如下所示:

上面的代码片段执行了以下操作:

*   导入所需的依赖项
*   使用`ConnectDB`函数创建一个`db`变量来初始化 MongoDB。
*   使用数据库逻辑中相应的功能修改`CreateProject`、`CreateOwner`、`Owners`、`Projects`、`Owner`和`Project`功能。

最后，我们需要用一个`bson:”_id”` struct 标签修改`models_gen.go`文件中生成的模型 id。我们使用 struct 标签重新格式化 MongoDB 返回的 JSON `_id`。

完成后，我们可以使用下面的命令启动开发服务器:

```
go run server.go
```

![](img/dbbe2217c13d6526d3a1e124746b7d1b.png)![](img/6f2ddd2d89842825b94be582c60fe011.png)![](img/66457170bdd0f3399dbf4d5a159d7e81.png)

我们还可以在 MongoDB 上验证操作。

![](img/40b2036b82bab6f5161cf97b29e29143.png)

# 结论

这篇文章讨论了如何使用 gqlgen 库和 MongoDB 用 Golang 构建一个项目管理应用程序。

这些资源可能会有所帮助:

*   [GraphQL 官方页面](https://graphql.org/)
*   [gqlgen GraphQL 库](https://github.com/99designs/gqlgen)
*   [MongoDB Go 驱动](https://docs.mongodb.com/drivers/go/current/)
*   [用 Golang 和 MongoDB 构建一个 REST API](https://dev.to/hackmamba/build-a-rest-api-with-golang-and-mongodb-gin-gonic-version-269m)