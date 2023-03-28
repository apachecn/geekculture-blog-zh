# 在 Angular 中使用 REST API 的一般方法

> 原文：<https://medium.com/geekculture/generic-approach-to-consume-rest-api-in-angular-a7d9c951f55e?source=collection_archive---------4----------------------->

![](img/179571b8c6696776d6cc52fd4c495565.png)

在本文中，我将向您展示如何创建一个通用的解决方案来使用 Angular 中的 REST API。我将利用 Typescript 泛型结合 Angular `HTTPClient`服务来消除任何代码冗余，尽可能做到[干燥](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)，并遵循[开闭原则](https://en.wikipedia.org/wiki/Open%E2%80%93closed_principle)。

# 使用 HTTPClient 与后端服务通信

大多数应用程序需要通过 HTTP 协议与远程服务器通信，以便执行基本的 [CRUD](https://en.wikipedia.org/wiki/Create,_read,_update_and_delete) 操作。有了 Angular，就可以使用`HTTPClient`服务轻松实现这种沟通。例如，如果您需要管理您的博客的帖子，您可以使用下面的服务来处理 Post 资源上的所有操作:

这个解决方案简单明了，甚至遵循了官方[角度文档](https://angular.io/guide/http)的最佳实践。然而，应用程序通常有许多资源需要管理，例如，我们可能有用户、评论、评论等。理想情况下，这些资源中的每一个都应该有一个单独的服务来处理 CRUD 操作并与服务器通信，最后我们会有`UserService`、`CommentService`、`ReviewService`。让我们看看`CommentService`会是什么样子:

# 问题是

尽管上述实现非常普遍且被广泛接受，但它有两个缺点:

*   代码冗余(打破了 DRY 原则):如果你比较一下`PostService`和`CommentService`，你会注意到代码有多冗余。
*   服务器端的改变，或者与服务器通信方式的改变，需要改变许多文件(在我们的例子中，我们需要改变`PostService`和`CommentService`文件)

# 拯救 Typescript 泛型

为了解决上述问题，让我们继续构建下面的抽象类，它将成为所有其他服务的基础:

*   新的服务类是`abstract`，这意味着它不能被实例化并直接使用，而是需要通过其他类来扩展。
*   我们提供了一个抽象方法`getResourceUrl`，扩展这个抽象类的类必须实现这个方法，并返回资源的 URL，我们将在下一节中看到。
*   这是一个泛型类，它不依赖于特定的类型，而是扩展这个抽象类的类将定义所使用的确切类型。
*   它拥有所有我们需要的 CRUD 操作，并在之前的服务中使用过。

现在，在我们有了抽象的泛型类之后，每当我们需要一个新的服务时，我们可以简单地扩展这个类并实现唯一的抽象方法`getResourceUrl`。因此 PostService 和 CommentService 将如下所示:

# 服务器与前端模型

在大多数应用程序中，前端模型与服务器端模型完全不匹配。换句话说，REST API 将使用与前端应用程序中定义的接口或类不完全匹配的 json 对象进行响应。在这种情况下，您需要一个映射函数在服务器和前端模式之间进行转换。这有时被称为序列化/反序列化。

因此，让我们扩展我们的基类来提供这种映射功能。为此，我更新了`ResourceService`,如下所示:

*   我添加了两个新方法:
*   `toServerModel`:为了从前端模型转换到服务器模型，它接受资源通用类型`T`并返回`any` (json)
*   `fromServerModel`:为了从服务器模型转换到前端模型，它接受一个代表服务器响应的`any`类型的参数，并返回通用类型`T`
*   我为两个方法`toServerModel`、`fromServerModel`都提供了一个默认的实现，所以在不需要映射的情况下，服务器返回的同一个对象将被用作前端模型。此外，由于我添加了一个默认的实现，这个服务的消费者根本不需要覆盖甚至实现这两个方法。
*   在`getList`和`get`方法中，我使用了新方法`fromServerModel`，将服务器响应映射到前端模型。
*   在`add`和`update`方法中，我使用`toServerModel`将前端模型映射到服务器模型，然后将数据发送到服务器。

现在，为了使用新的变化，我们有两种情况:

1.  服务器和前端模型之间不需要映射，在这种情况下，我们不需要在扩展`resourceService`的类中做任何修改。
2.  在服务器和前端模型之间需要某种映射，我们需要做的就是在派生类中重写`toServerModel`和`fromServerModel`模型来解决我们的需求映射。例如，假设之前实现的`PostsService`需要从时间戳映射到 js Date 对象，PostsService 实现如下所示:

# 结论:

要使用 HTTP 协议与服务器通信，您需要使用 Angular HTTPClient 服务。在本文中，我们实现了一个通用的可扩展解决方案来实现这种通信。我们的解决方案是干净的，[干的](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)，并遵循[开合原理](https://en.wikipedia.org/wiki/Open%E2%80%93closed_principle)。我们利用了 Typescrip 泛型、泛型类，我们甚至考虑了服务器和前端模型之间所需的映射。