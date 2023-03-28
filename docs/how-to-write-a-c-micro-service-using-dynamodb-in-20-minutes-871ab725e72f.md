# 如何在 20 分钟内用 DynamoDB 写一个 C++微服务

> 原文：<https://medium.com/geekculture/how-to-write-a-c-micro-service-using-dynamodb-in-20-minutes-871ab725e72f?source=collection_archive---------15----------------------->

![](img/84522886148fbd4a44d79fd893ee8baa.png)

Photo by [Anthony Indraus](https://unsplash.com/@aindraus?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

编写可伸缩且健壮的服务和应用程序需要一个可伸缩且健壮的解决方案来存储、搜索和查询数据。数据库市场提供了许多不同的选择。几个关键因素将一个数据库与另一个数据库区分开来，以及为什么特定的应用程序或服务选择它所使用的数据库。大多数情况下，选择数据库的最关键因素是部署和扩展它的难易程度。部署数据库所需的时间长度将限制服务或应用程序最初启动的速度，或未来采用新版本的速度。

Amazon Web Services 提供的 DynamoDB 是一个 noSQL 数据库，它提供动态吞吐量和伸缩性、一致的读写，以及轻松的表加速和停止。DynamoDB 使用一个分区和排序键系统，它允许操作以介于传统关系 SQL 数据库和 MongoDB 等基于 noSQL 文档的数据库之间的方式进行。DynamoDB 允许对象的结构和内容具有灵活性，具有多种数据类型，如字符串、数字、集合、地图等等。

本文将介绍如何在很短的时间内使用 C++、AWS 软件开发工具包和 DynamoDB 编写一个基本的微服务。假定您对 C++和 CMake 语言都有基本的了解。此外，要运行这里给出的代码，还需要一个 AWS 帐户，以及一个拥有访问权和密钥的 IAM 角色。

# 设置和安装

为了构建 C++程序，您需要一个 C++编译器，以及一个构建系统生成器。对于这种情况，将在 MacOS 上使用 clang 和 CMake。然而，AWS SDK for C++可以在 Windows、Linux、MacOS 甚至 Android 上编译和运行。此处有一个包含本指南所有示例代码的资源库。

首先，用于实现该项目的目录应该如下所示:

```
- <project directory>
     |
     - CMakeLists.txt
     - main.cpp
```

在这个目录中，`CMakeLists.txt`文件将在构建时从 Github 下载 AWS SDK，在本地依赖目录中构建它，然后安装库和头文件。因为 DynamoDB 是本演示中将使用的唯一 Amazon web 服务，所以我们将限制 CMake 中传递的选项，只构建与联系 DynamoDB 相关的代码。AWS SDK 是一个使用[组件](https://cmake.org/cmake/help/latest/command/find_package.html#basic-signature-and-module-mode)的 CMake 项目，这是一个 CMake 特性，它允许指定只应该找到更大项目的某些库并链接到另一个目标可执行文件中。

CMake 文件的主要部分是以下语句:

`ExternalProject_Add`调用告诉 CMake 我们想要在 Github 上构建和使用一个项目。这个函数允许我们传递 CMake 选项来指定何时构建外部项目，比如不构建或运行单元测试，以及构建静态库。根据您的特定操作系统发行版，在构建 SDK 时，可能会下载、构建和安装更多的库。这是因为您可能没有安装像 curl 或 openssl 这样的常用库。

为了简化将 SDK 库链接到我们简单的可执行文件中的过程，库和 include 目录一起被明确列出。如果您在 windows 上尝试这样做，您将需要从列表中删除`pthread`库。

既然我们已经确定了如何设置和安装项目的依赖项，那么让我们来理解将用于编写 DynamoDB 应用程序的 SDK

## AWS SDK

AWS SDK for C++将每个服务的代码分为三个不同的组。首先是模型。这些对象以与正在使用的服务相关的格式存储数据。例如，有了 DynamoDB，你就有了一个`AttributeValue`。这是一个包含 DynamoDB 支持的不同数据类型的对象。例如，如果您想将值设置为一个`string`类型，您可以使用方法`SetS`。如果您想检索字符串值，您可以使用`GetS`。

第二个是请求。在调用 Amazon web 服务之前，请求包含需要打包到一个对象中的所有信息。对于 DynamoDB，`UpdateItemRequest`至少需要被更新的项的分区键，`UpdateItemExpression`，一种表示要更新的项的属性的查询，以及提供更新表达式的值。

第三是结果和结果。SDK 中 Amazon web 服务的每个响应都会给你一个结果。结果包含一个结果和一个潜在的错误对象。result 对象包含了您期望在您可能使用的特定 API 的响应中找到的所有内容。使用 DynamoDB，`ScanResult`将把扫描表格的“结果”打包在一起。

# 创建和删除表格

表是使用 DynamoDB 的支柱和最大的组成部分。您必须先创建一个表，然后才能在 DynamoDB 中执行任何操作或存储数据。为此，您可以通过 AWS 控制台创建一个表。然而，为了简化本教程中编写的可执行文件，让我们通过 AWS SDK 代码创建和删除一个表。

为了在 C++程序中使用 SDK，必须首先初始化 SDK。这是必需的，因为 SDK 使用 curl 库来形成连接池，它可以使用连接池来连接 AWS。为简单起见，SDK 的初始化和关闭将封装在 RAII 对象中，如下所示:

*警告:在向 AWS 发出请求之前未能初始化 SDK 将导致分段错误。*

我们将编写的与 DynamoDB 通信的代码将封装在一个`ScopedDynamoTable`类中。这将是一个 RAII 样式的对象，其中表的存在仅限于 C++范围。该类的构造函数将创建该表，并等待它变为活动状态，而析构函数将删除该表。

首先，让我们看看该类的构造函数是如何创建表的:

首先，用所有默认配置初始化 DynamoDB 客户端类。这存储在 SDK 提供的一个特殊的唯一指针中，为了与 SDK 的定制内存管理系统兼容，建议在`std::unique_ptr`上使用该指针。之后，使用指定的名称、散列(分区)键以及我们想要为表指定的吞吐量构建一个`CreateTableRequest`。虽然 DynamoDB 支持按需吞吐量和预配吞吐量，但是为了让这个示例程序运行起来几乎不花费任何成本，我们将使用一个小的预配吞吐量。关于 DynamoDB 吞吐量和供应的更多信息可以在[这里](https://aws.amazon.com/dynamodb/pricing/)找到。与此类似，这个类的析构函数将使用一个`DeleteTableRequest`，只需要指定要删除的表的名称。

注意在请求的结果上使用`IsSuccess()`是很重要的。这是用于几乎所有 SDK 请求的方法，并且是一种通用的布尔方法来确定它们是否成功。

# 添加项目

既然我们可以创建 DynamoDB 表，我们需要能够向表中添加条目。向 DynamoDB 中的表添加一个条目是通过封装在`PutItemRequest`对象中的`PutItem`操作来完成的。添加到表中的项必须至少包含分区键和相应的值。除此之外，同一表中的项可以有不同的键和值。因为我们的分区键是一个字符串，所以我们将只添加一个包含分区键和值的项。因此，将项目放入表中的方法应该如下所示:

在这段代码中，`PutItemRequest`对象用于格式化一个项目，发送到 DynamoDB 以添加到我们的表中。不过这里用了一个`ConditionExpression`。在 DynamoDB 中，每个基于项目的请求也可能有一个条件表达式。该条件允许请求只有在条件为真时才会成功，否则，请求将失败，并显示一个特定的失败代码，指示它只是由于条件为假而失败。条件表达式是 DynamoDB 非常有用的一部分，因为它们允许对 DynamoDB 的请求是事务性的。带有条件表达式的请求具有读-修改-写语义，这些语义对于作用于表上的其他请求来说是原子性的。

# 阅读和打印项目

除了向我们的表中添加项目，我们还希望能够读取这些项目，并在我们的服务中对它们做一些事情。DynamoDB 支持两种不同的从表中读取项的操作。第一个也是最直接的方法是扫描。扫描从表中读取所有项目，可以选择提供过滤表达式以及对返回项目属性的限制。另一种方法是查询，它更有效，因为它可以采用一个表达式，该表达式只能根据分区键或附加的二级索引从表的特定部分读取数据。用于扫描表的筛选表达式仅在读取整个表后筛选项目。对于大型表，通常不建议扫描。然而，为了简单起见，我们将扫描表来读取项目。

在 SDK 代码中，`ScanRequest`对象，类似于目前使用的其他请求对象，可以被构造和构建来扫描表。为了演示，我们可以实现一个名为`printItems`的方法，该方法扫描表并打印出每一项的键和值。

对于包含从表中读取的项的 SDK for DynamoDB 中的任何结果对象，这些项以映射的形式给出，在键名和属性值之间。虽然这里使用了`auto`类型来简化代码和类型名称，但是`GetItems()`调用将返回一个`const Aws::Vector<Aws::Map<Aws::String, AttributeValue>>&`，一个地图向量，其中每个地图代表一个项目。对于除 android 以外的任何平台，`Aws::Vector`和`Aws::Map`分别与`std::vector`和`std::map`相同。

如果我们用一组有效的 AWS 凭证将目前所有的代码放在一起，编译并运行它，您应该会在终端上看到下面的输出。这里使用的是位于[这里](https://github.com/jweinst1/dynamo-db-cpp)的整体回购项目。

```
Table: 'schedule' created successfully
Waiting until table is ready....
Waiting until table is ready....
Waiting until table is ready....
Waiting until table is ready....
Waiting until table is ready....
Waiting until table is ready....
Waiting until table is ready....
Waiting until table is ready....
Waiting until table is ready....
Waiting until table is ready....
Scanned a total of 3 items
{event : lunch}
{event : morning}
{event : basketball}
Table "schedule" has been deleted
* Closing connection 0
```

总的来说，DynamoDB 是 C++程序员创建微服务和应用程序的一个很好的选择。它是可伸缩的，易于配置，并且可以在很短的时间内设置为与现有的 C++项目一起工作。