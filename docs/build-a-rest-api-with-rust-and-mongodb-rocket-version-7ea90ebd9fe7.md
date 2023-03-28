# 用 Rust 和 MongoDB 构建一个 REST API 火箭版

> 原文：<https://medium.com/geekculture/build-a-rest-api-with-rust-and-mongodb-rocket-version-7ea90ebd9fe7?source=collection_archive---------2----------------------->

![](img/673d3b7000bdc7d4f750334420607aa3.png)

cover image

REST API 已经成为从一个数据源到另一个数据源连接和传输数据的事实。它为设计和开发 web 服务提供了一套指导方针和架构模式。

这篇文章将讨论使用 [Rocket](https://rocket.rs/) 框架和 [MongoDB](https://www.mongodb.com/) 用 Rust 构建一个用户管理应用程序。在本教程的最后，我们将学习如何构建一个 Rust 应用程序，构建一个 REST API 并使用 MongoDB 持久化我们的数据。

Rocket 是一个用 Rust 编写的 HTTP web 框架，优先考虑安全性、灵活性和速度。Rocket 附带了类型安全、可重用性、日志、静态文件服务等特性，开发人员可以利用这些特性用更少的代码构建可伸缩的应用程序。

MongoDB 是一个基于文档的数据库管理程序，用作关系数据库的替代方案。MongoDB 支持处理大型分布式数据集，并提供无缝存储或检索信息的选项。

完整的源代码可以在这个[库](https://github.com/Mr-Malomz/rocket-mongo-api)中找到。

# 先决条件

要完全掌握本教程中介绍的概念，需要有 Rust 方面的经验。使用 MongoDB 的经验不是必需的，但是拥有它是很好的。

我们还需要以下物品:

*   一个 [MongoDB 帐户](https://www.mongodb.com/)来托管数据库。 [**报名**](https://www.mongodb.com/cloud/atlas/register) **完全免费**。
*   [Postman](https://www.postman.com/downloads/) 或任何 API 测试应用

# 让我们编码

## 入门指南

首先，我们需要导航到所需的目录，并在我们的终端中运行下面的命令

```
cargo new rocket-mongo-api && cd rocket-mongo-api
```

该命令创建一个名为`rocket-mongo-api`的 Rust 项目，并导航到项目目录。

接下来，我们通过修改`Cargo.toml`文件的`[dependencies]`部分来安装所需的依赖项，如下所示:

`rocket = {version = “0.5.0-rc.2”, features = [“json”]}`是一个基于 Rust 的框架，用于构建 web 应用。它还指定了所需的版本和特性类型(json)。

`serde = “1.0.136”`是一个序列化和反序列化 Rust 数据结构的框架。例如，将 Rust 结构转换为 JSON。

`dotenv = “0.15.0”`是一个管理环境变量的库。

`[dependencies.mongodb]`是连接 MongoDB 的驱动。它还指定了所需的版本和特性类型(同步 API)。

我们需要运行下面的命令来安装依赖项:

```
cargo build
```

# 应用程序入口点

安装好项目依赖项后，将`src`文件夹中的`main.rs`文件修改如下:

上面的代码片段执行了以下操作:

*   导入所需的依赖项
*   创建一个`hello`处理程序，它使用 Rust 宏来指定 HTTP 方法、路由路径`/`，并返回一个`JSON of Hello from rust and mongoDB`。
*   使用`#launch macro`异步运行`main`函数并启动服务器。`main`函数使用 build 函数创建一个新的服务器，并将`hello`处理程序安装到一个路由中。

接下来，我们可以通过在终端中运行下面的命令来测试我们的应用程序。

```
cargo run
```

Rocket 在`[http://127.0.0.1:8000](http://127.0.0.1:8000)`或`localhost:8000`上运行开发服务器。

![](img/e064b945acbb96a6d9b39732ae48678d.png)

# 生锈的模块系统

Rust 中的模块是一种将代码分割成可重用组件并管理它们之间可见性的机制。模块帮助我们为我们的项目保持一个良好的项目结构。

为此，我们需要导航到 src 文件夹，并使用相应的`mod.rs`文件创建`api`、`models`和`repository`文件夹来管理可见性。

![](img/25e746524988f357743470e246194d8a.png)

`api`用于模块化 API 处理程序。

`models`用于模块化数据逻辑。

`repository`用于模块化数据库逻辑。

**添加对模块** 的引用为了使用模块中的代码，我们需要将它们声明为一个模块，并导入到`main.rs`文件中。

# 设置 MongoDB

完成后，我们需要登录或注册我们的 [MongoDB](/) 账户。点击项目下拉菜单并点击**新项目**按钮。

![](img/bec1e00a02e34655425911798e7a0b94.png)

输入`rust-api`作为项目名称，点击旁边的**，点击**创建项目..****

![](img/656910d44e7c8139c8351b7b027dfe23.png)![](img/67e4343f37a876e9edc443b83ada0fc4.png)

点击**建立数据库**

![](img/8f8c62a66a9136a280bb3edeb2dcb099.png)

选择**共享**作为数据库类型。

![](img/5604fa07795a3a2b5f3b5d366c3ee9b2.png)

点击**创建**以设置集群。这可能需要一些时间来设置。

![](img/e34447fdeaed7c3a932426480e1ffa8d.png)

接下来，我们需要创建一个用户，通过输入**用户名**、**密码**，然后点击**创建用户**，从外部访问数据库。我们还需要添加我们的 IP 地址，通过点击**添加我当前的 IP 地址**按钮安全地连接到数据库。然后点击**完成并关闭**保存更改。

![](img/e8b232b90a9876d44eedde2ace8c1d78.png)![](img/d1fa635223987510b53b9f5c1c1f4829.png)

保存更改后，我们应该会看到一个数据库部署屏幕，如下所示:

![](img/34721ece9a6ca71a51c2db40099c4ed6.png)

# 将我们的应用程序连接到 MongoDB

配置完成后，我们需要将应用程序与创建的数据库连接起来。为此，点击**连接**按钮

![](img/23d16cedec9d7d45416eb2636e366cf4.png)

点击**连接你的应用**，将**驱动**改为 Rust，并将**版本**如下图所示。然后点击复制图标**复制连接字符串。**

![](img/8e41d2a0d9bb4e22921a20b67f62d236.png)![](img/added3bed279021456a5b1633ea0d718.png)

**设置环境变量**

接下来，我们必须用之前创建的用户密码修改复制的连接字符串，并更改数据库名称。为此，首先，我们需要在根目录下创建一个`.env`文件，并在这个文件中添加复制的代码片段:

```
MONGOURI=mongodb+srv://<YOUR USERNAME HERE>:<YOUR PASSWORD HERE>@cluster0.e5akf.mongodb.net/myFirstDatabese?retryWrites=true&w=majority
```

下面是正确填充的连接字符串示例:

```
MONGOURI=mongodb+srv://malomz:malomzPassword@cluster0.e5akf.mongodb.net/golangDB?retryWrites=true&w=majority
```

# 创建 REST APIs

设置完成后，我们需要创建一个模型来表示我们的应用程序数据。为此，我们需要导航到`models`文件夹，在这个文件夹中，创建一个`user_model.r`文件，并添加下面的代码片段:

上面的代码片段执行了以下操作:

*   导入所需的依赖项
*   使用`derive`宏生成对格式化输出、序列化和反序列化数据结构的实现支持。
*   创建一个具有所需属性的`User`结构。我们还向`id`属性添加了字段属性，以便重命名并在字段为空时忽略它。

**PS**:*`*pub*`*修饰符使结构及其属性成为公共的，可以从其他文件/模块中访问。**

*接下来，我们必须将`user_model.rs`文件注册为`models`模块的一部分。为此，打开`models`文件夹中的`mod.rs`并添加以下代码片段:*

***创建一个用户端点** 模型完全建立并可供使用后，我们现在可以创建我们的数据库逻辑来创建一个用户。为此，首先，我们需要导航到`repository`文件夹，在这个文件夹中，创建一个`mongodb_repo.rs` 文件并添加下面的代码片段:*

*上面的代码片段执行了以下操作:*

*   *导入所需的依赖项*
*   *创建一个带有`col`字段的`MongoRepo`结构来访问 MongoDB 集合*
*   *创建一个向`MongoRepo`结构添加方法的实现块*
*   *将一个`init`方法添加到实现块，以加载环境变量，创建到数据库的连接，并返回一个`MongoRepo`结构的实例*
*   *添加一个`create_user`方法，该方法接受一个`self`和`new_user`作为参数，并返回创建的用户或一个错误。在方法内部，我们使用`User`结构创建了一个新文档。然后我们使用引用`MongoRepo`结构的`self`来访问集合中的`insert_one`函数，以创建一个新用户并处理错误。最后，我们返回了创建的用户信息。*

***PS:** *新建文档时指定的* `*None*` *告诉 MongoDB 自动生成用户的* ***id*** *。**

*接下来，我们必须将`mongodb_repo.rs`文件注册为存储库模块的一部分。为此，打开存储库文件夹中的`mod.rs`,并添加下面的代码片段:*

*其次，我们需要创建一个处理程序，它使用来自`repository`的`create_user`方法来创建一个用户。为此，我们需要导航到`api`文件夹，在该文件夹中，创建一个`user_api.rs`文件并添加以下代码片段:*

*上面的代码片段执行了以下操作:*

*   *导入所需的依赖项*
*   *使用路由宏指定 HTTP 方法、路由，并指示处理程序需要正文数据*
*   *创建一个接受`db`、`MongoRepo`的类型和`new_user`作为参数的`create_user`处理程序。在处理程序内部，我们创建了一个用于创建用户的`data`变量，使用`db.create_user`方法将其插入到数据库中，如果插入成功，返回正确的响应，如果有错误，则返回错误。*

***PS:** *用于定义参数的* `*&State*` *和* `*Json*` *结构分别用于管理跨路由共享的应用状态和从请求净荷中提取 JSON 数据。**

*最后，我们需要修改我们的应用程序入口点，以包含`create_user`处理程序。为此，我们需要导航到`main.rs`文件并修改它，如下所示:*

*上面的代码片段执行了以下操作:*

*   *导入所需的依赖项*
*   *创建一个`db`变量，通过调用`init()`方法建立到 MongoDB 的连接，并将其添加到 manage 函数中，使数据库状态在整个应用程序范围内可用*
*   *使用`app_data`和`service`函数将应用程序数据和处理程序添加到 App 实例中*

***获取一个用户端点** 要获取一个用户的详细信息，我们必须首先通过向实现块添加一个`get_user`方法来修改`mongodb_repo.rs`文件。*

*上面的代码片段执行了以下操作:*

*   *修改依赖关系以包括`oid::ObjectId`和`doc`*
*   *添加一个`get_user`方法，它接受一个`self`和`id`作为参数，并返回用户详细信息或一个错误。在该方法中，我们将`id`转换为`ObjectId`，并将其用作`filter`来获取匹配的文档。然后我们使用引用`MongoRepo`结构的`self`从集合中访问`find_one`函数，以获取用户的详细信息并处理错误。最后，我们返回了创建的用户信息。*

*其次，我们需要通过创建一个处理程序来修改`user_api.rs`，该处理程序使用来自`repository`的`get_user`方法来获取用户。*

*上面的代码片段执行了以下操作:*

*   *使用路由宏指定 HTTP 方法、相应的路由和路由参数*
*   *创建一个`get_user`处理程序，它接受`db`、一个到`MongoRepo`的类型和一个用于访问路由路径的`path`作为参数。在处理程序内部，我们创建了一个`id`变量来获取用户的 **id** ，使用`db.get_user`方法从数据库中获取用户的详细信息。如果请求成功，我们返回正确的响应，如果有错误，则返回错误。*

*最后，我们需要修改我们的应用程序入口点(main.rs ),通过导入处理程序并为其添加一个新服务来包含 get_user 处理程序。*

***编辑用户端点*** 

*上面的代码片段执行了以下操作:*

*   *修改依赖关系以包括`UpdateResult`*
*   *添加一个`update_user`方法，该方法接受一个`self`、`id`和`new_user`参数，并返回更新的用户详细信息或一个错误。在该方法中，我们将`id`转换为`ObjectId`，创建了一个`filter`变量来获取我们想要更新的匹配文档，并使用`doc`宏来更新文档字段。然后，我们使用引用`MongoRepo`结构的`self`来访问集合中的`update_one`函数，以更新与指定的`filter`匹配的用户并处理错误。最后，我们返回了更新后的用户信息。*

*其次，我们需要通过创建一个处理程序来修改`user_api.rs`，该处理程序使用存储库中的`update_user`方法来更新用户。*

*上面的代码片段执行了以下操作:*

*   *修改依赖关系以包括`ObjectId`*
*   *使用路由宏指定 HTTP 方法、相应的路由、路由参数和正文数据*
*   *创建一个`update_user`处理程序，它接受`db`、`MongoRepo`、`path`和`new_user`的类型作为参数。在处理程序内部，我们创建了一个`id`变量来获取用户的 **id** ，通过传递更新后的用户信息，使用`db.update_user`方法从数据库中更新用户的详细信息。最后，我们检查更新是否成功，并返回更新后的用户或错误(如果有)。*

*最后，我们需要修改我们的应用程序入口点(`main.rs`)，通过导入处理程序并为其添加新服务来包含`update_user`处理程序。*

***删除一个用户端点***

*要删除一个用户，我们必须首先通过向实现块添加一个`delete_user`方法来修改`mongodb_repo.rs`文件。*

*上面的代码片段执行了以下操作:*

*   *修改依赖关系以包括`DeleteResult`*
*   *添加一个`delete_user`方法，该方法接受一个`self`和`id`作为参数，并返回删除的用户详细信息或一个错误。在该方法中，我们将`id`转换为`ObjectId`，并创建了一个`filter`变量来获取我们想要删除的匹配文档。然后我们使用引用`MongoRepo`结构的`self`来访问集合中的`delete_one`函数，以删除与指定的`filter`匹配的用户并处理错误。最后，我们返回了删除的用户信息。*

*其次，我们需要通过创建一个使用`repository`中的`delete_user`方法删除用户的处理程序来修改`user_api.rs`。*

*上面的代码片段执行了以下操作:*

*   *使用路由宏指定 HTTP 方法、相应的路由和路由参数*
*   *创建一个`delete_user`处理程序，它接受`db`、`MongoRepo`和`path`的类型作为参数。在处理程序内部，我们创建了一个`id`变量来获取用户的 **id** 和使用`db.delete_user`方法通过传入`id`从数据库中删除用户。最后，我们返回适当的响应或错误(如果有的话)。*

*最后，我们需要修改我们的应用程序入口点(`main.rs`)，通过导入处理程序并为其添加一个新的`service`来包含`delete_use` r 处理程序。*

***获取所有用户端点***

*要获得用户列表，我们必须首先通过向实现块添加一个`get_all_users`方法来修改`mongodb_repo.rs`文件。*

*上面的代码片段添加了一个 g `et_all_users`方法，该方法接受一个`self`作为参数，并返回用户列表或一个错误。在方法内部，我们使用引用`MongoRepo`结构的`self`从集合中不带任何`filter`地访问`find`函数，以便它可以匹配数据库内部的所有文档并处理错误。最后，我们返回了用户列表。*

*其次，我们需要通过创建一个处理程序来修改`user_api.rs`，该处理程序使用存储库中的`get_all_users`方法来获取用户列表。*

*上面的代码片段执行了以下操作:*

*   *使用路由宏指定 HTTP 方法和相应的路由*
*   *创建一个 get_all_users 处理程序，它使用`db.delete_user`方法来获取用户列表。然后，我们返回用户列表或错误(如果有的话)。*

*最后，我们需要修改我们的应用程序入口点(`main.rs`)，通过导入处理程序并为其添加一个新的`service`来包含`get_all_users`处理程序。*

*完成后，我们可以通过在终端中运行下面的命令来测试我们的应用程序。*

```
*cargo run*
```

*![](img/42d363b52d44e83cc3669ccd750b67b6.png)**![](img/9974cd05bbce2cc5bbe1bcf84140df94.png)**![](img/a9e4358b45e2c1933cec8c674d90e4fa.png)**![](img/ef60dbae07ad435d17da8db0d9349029.png)**![](img/5f2b49ce2eaae60ed1b02b6a59afd7a1.png)**![](img/0f392748762718be0663bf90927b2bcb.png)*

# *结论*

*这篇文章讨论了如何模块化 Rust 应用程序，构建 REST API，并使用 MongoDB 持久化我们的数据。*

*您可能会发现这些资源很有帮助:*

*   *[火箭](https://rocket.rs/)*
*   *[MongoDB Rust 驱动](https://github.com/mongodb/mongo-rust-driver)*
*   *[Serde(序列化和反序列化库)](https://serde.rs/)*
*   *[铁锈环境装载机](https://crates.io/crates/dotenv)*