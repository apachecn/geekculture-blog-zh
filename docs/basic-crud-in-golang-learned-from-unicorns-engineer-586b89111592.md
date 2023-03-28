# 从独角兽的工程师那里学到的 Golang 的基本 CRUD

> 原文：<https://medium.com/geekculture/basic-crud-in-golang-learned-from-unicorns-engineer-586b89111592?source=collection_archive---------7----------------------->

![](img/6afc4438c9319cd0e35653c6bc75f28f.png)

just a cute illustration

我已经为许多项目和后端技术工作了近 10 年，在过去的 3 年里，我还尝试将 Node JS 作为我的后端堆栈之一。现在我想重点谈谈 Golang 的使用，因为 Golang 有着良好的记录，许多初创公司都在使用它作为后端堆栈。

为了帮助我更深入地学习 Golang，我决定购买 Golang 课程，幸运的是，我的导师是印度尼西亚一家独角兽公司的工程师。就我个人而言，当我第一次使用 Golang 创建一个简单的项目时，我觉得这个堆栈是如此的严格和复杂。但从记忆和速度上知道结果后，我对这种语言印象深刻。所以在这篇文章中，我将写一个教程来使用 Golang 制作 CRUD API，这可能是一个很长的教程，因为我想一步一步地写。

# 装置

**首先**，你需要做一个空文件夹(假设文件夹名是 **go-basic-crud** )然后去那个文件夹。
接下来，在终端中，您需要使用该命令设置初始模式

command to init mod Golang project

之后，我们需要安装 [**Gin Gonic**](https://github.com/gin-gonic/gin) 使我们的 Golang 可以使用请求访问。只需在终端中键入以下命令

command to install gin-gonic libary

安装的最后一步是我们需要创建一个 **main.go** 文件。在该文件中，调用 gin-gonic 并创建一个路由器

![](img/98420a37bf0307b292cbf06d2a26ee0d.png)

main.go

最后，我们可以通过运行项目来测试我们的安装。使用 **go run main.go** 运行，当您从浏览器访问 **/ping** 时，会显示 json 响应。

# **插入数据库**

在去编码之前，别忘了创建一个新的数据库(假设名字是 **go-basic** )。我们不打算创建表，因为我们将使用自动迁移来创建表。

![](img/068071b6373448f0e2ed374b79d59d19.png)

task’s table schema

接下来，我们将制作一些文件和文件夹来维护我们的代码，如下所示

![](img/05fbaa3340b72081c329aea763a40973.png)

folder structure

首先，我们在任务文件夹中创建一个**实体。这个文件特意定义了我们的表模式，所以这个文件将用于自动迁移和 ORM。**

![](img/a36009ee4ac3cbba86334ae0397ff72f.png)

entity.go

第二个文件我们需要在 **input.go** 中定义输入为 struct。

![](img/88868db6573afd864cab15b2f9b018f5.png)

input.go

我们需要制作的第三个文件是 repository.go，它用于操作我们的数据库记录。别忘了先安装 [**GORM**](https://gorm.io/) 。

![](img/6bda95d1d64aff257f3eb554eab8b28d.png)

repository.go

下一个文件是 **service.go** ，这个文件的目的是调用存储库函数，服务文件内部的函数可能会被其他域调用。

![](img/13ff8d2a2b1b66df3affff981d04ccc1.png)

service.go

最后一个文件是处理程序文件夹中的 **task.go** 。这个文件的目的是让路由器调用函数和使用服务函数，它是一种控制器文件。

![](img/55550cf3f43f9b4dc4429a2d778b0e42.png)

task.go

本节最后一步是定义我们的数据库连接，进行自动迁移，定义存储库、服务和处理程序，然后创建路由。

![](img/1089566b42da5cb61f9c39bb4cb12c16.png)

main.go

重新启动我们的服务器后，您可以尝试点击/task 从任务表中获取所有记录。

# 从数据库获取所有数据

实际上，我们已经为存储数据做了很长的一步，现在我们将继续获取数据。

首先，我们需要在**存储库中创建一个新函数。我们给函数起个名字 **SelectAll()** 。**

![](img/2a1af2f76cd494da566ea03f18a49a87.png)

repository.go

为了调用存储库中的函数，我们将在 **service.go** 文件中调用它。让我们创建一个名为 **Index()** 的新函数。

![](img/a48b648bed4c87d6d6ffd942f9f487b9.png)

service.go

接下来，调用**任务中的服务函数。让我们创建名为**索引为**的新函数。**

![](img/950a765e228f4abf04608230c199bee0.png)

task.go

最后一点是，我们只需要在 **main.go** 中注册新的路线。

![](img/132e924e1c44bfd806ee0e2a9776a173.png)

main.go

# 按 ID 获取数据

对于创造这种流动，我们不会做太多的改变。那我们开始吧。

首先，我们在**输入中需要一个新的结构。只需调用 **InputTaskDetail** struct。**

![](img/4a6e813db3566f787b2f65ce5a6417d0.png)

input.go

接下来，我们当然需要在**存储库中创建一个新函数。只需调用 **SelectById** 。**

![](img/e79b24b49ad0ff68f83ba903629f560c.png)

repository.go

调用**服务中的新功能。创建名为 **SelectById** 的新函数。**

![](img/e79b24b49ad0ff68f83ba903629f560c.png)

service.go

在 handler 中调用新服务的函数，这样我们就可以创建一个新的路由。让我们把它命名为**显示为**。

![](img/572495c8e69a5b002a2dfacb4e708671.png)

task.go

最后一步是在 **main.go** 中创建一条新路线，这样我们就可以通过 ID 获取记录。

![](img/f41777c3f5cd9d6b78edc096db413241.png)

main.go

重新启动您的服务器，并使用/task/:id(例如 localhost:8080/api/task/1)进行访问以获取数据。

# 更新数据

下一部分是更新数据，因为它与按 ID 选择数据相关。

在**库中创建一个新函数，用名称**转到**，更新**。

![](img/6461c0450e363e7f5b30530187ca1e81.png)

repository.go

移动到 **service.go** 文件，创建一个名为 **Update** 的新函数。

![](img/e1e3b4d58fc332e32e2a93dff799a2f7.png)

service.go

然后在我们的处理程序中创建一个新函数来允许路由更新数据。

![](img/14c0fbfee97ab27e0efd3a6665c841ad.png)

task.go

最后一次触摸，在 **main.go** 中注册新路线。

![](img/e216ecfefc3fca8f89e0914d77401275.png)

main.go

为了测试我们的新路线，不要忘记首先重启服务器。

# 按 ID 删除数据

实际上，这个步骤与更新数据没有什么不同，只是在存储库、服务、处理程序中创建一个新的函数，并注册新的路由。

在 **repository.go** 中，创建一个名为 **Destroy** 的新函数。

![](img/6dffe6b9af60533e1174a8cfb23a7313.png)

repository.go

同样在 **service.go** 中，创建一个名为 **Destroy** 的新函数。

![](img/5e1ea98a1452f861724afa6f82fa6aa7.png)

service.go

在处理程序文件中，创建一个名为 **Destroy** 的新函数。

![](img/ad63cf60e1fcd28c17625bd335bd9ecc.png)

service.go

最后一次触摸是像往常一样在 **main.go** 中注册一条新路线。

![](img/e687ba35acca6bc7f96b42431eb88e29.png)

main.go

重启你的服务器，点击我们刚刚创建的新 API。

![](img/c250c0ea0987b39a5352479f55ddd559.png)

ganbatte!!

# **结论**

对于第一次用 Golang 创建 CRUD API 的我来说，感觉是如此的严格和复杂，我们需要创造条件来处理 ***err*** 。但是这个语言很好，我们可以同时做更多的逻辑和并发请求。我们也可以在任何操作系统中使用编译后的项目。

如果你想克隆这个项目，我已经提供了我的 [**Github**](https://github.com/taufanfadhilah/GO-basic-crud) 。请随时给我发信息，也许你有问题或想合作，只需通过我的 [**LinkedIn**](https://www.linkedin.com/in/taufanfadhilahiskandar/) 。谢谢！