# 创建 API 的新方法

> 原文：<https://medium.com/geekculture/a-new-approach-to-create-apis-8fe84e158b4c?source=collection_archive---------34----------------------->

![](img/9fd334c641a88cf56abc4688b6310b74.png)

[https://pixabay.com/photos/greece-parthenon-temple-ruins-1594689/](https://pixabay.com/photos/greece-parthenon-temple-ruins-1594689/)

我想提出一个非常简单的问题。

> ***有没有更快创建 API 的新方法？***

# 经典的方式

通常，我们使用一些框架来创建 API，即使它们实际上是 MVC 框架。或者，如果您是 Node.js 开发人员，可以从简单的 Express 服务器开始。我们可以选择许多不同的库和工具。但是，在开发 API 时，我们有两个不同的任务；实现业务逻辑，一遍又一遍地编写相同的代码。

这么多年后，我问自己，我能不能创建一个健壮的结构来处理 API 的所有公共特性。我的意思是，不同的方式或方法…

# 相同和不同的东西

让我们想想你在职业生涯中创造的 API。很可能，他们有一些共同的模式。至少，一个实体——一个用户实体——应该有基本的 CRUD 操作。此外，我很确定在某个地方你需要一些扩展的查询功能。但事情并不简单。有几种设计模式可以用在 API 设计上。我们正在尽可能地实现它们，这样我们就能有好的、可靠的 API。

然而，没有人会使用相同的 API，因为我们有不同的业务逻辑。因此，我们应该在某个地方设置一个断点，将业务逻辑和共享功能分开。

经过这些思考，我有了一个想法，目前我正在努力。

# 首先定义

让我们考虑一个用户实体。对于那个实体，你可能想要不同的东西。例如，您可能需要以下功能:

*   创建一个简单的 CRUD
*   仅允许创建和更新请求的特定字段。
*   使用一些表单验证来确保用户发送了正确的数据。
*   对用户隐藏一些秘密数据，如密码散列。
*   开发扩展查询功能。
*   在创建过程中应用一些特殊的业务逻辑。
*   等等。

你可以在这个列表中添加更多的东西，但这足以理解我的想法。要为用户实体创建 API，让我们创建一个模型文件。

```
class User {
  get fillable() {
    return ["email", "name"];
  }

  get validations() {
    return {
      email: "required|email",
      name: "required",
    };
  }
}
```

这不是 ORM 模型。这只是我们想要的默认特性的定义。如果在你创建了那个模型之后，你可以得到完全工作的 API，仅仅是根据你的定义，会怎么样呢？

嗯，我已经工作了很长时间来创造这样的东西。它被命名为 [Axe API](https://axe-api.github.io/) ，一种快速创建 Rest APIs 新方法。

Axe API 希望您提供模型定义。 *Axe API* 提供了一个健壮的、有效的 API，当你定义模型的特性时，比如验证规则、可填充的字段、选定的处理程序(CRUD)以及彼此之间的关系。但不仅仅如此。它为您在 HTTP 请求的每一步中实现业务逻辑提供了许多转义点。神奇的结果是，您可以为您拥有的每个模型提供非常扩展的查询功能。

# 入门指南

让我们仔细看看，想出这样一个简单的模型；

```
import { Model } from "axe-api";

class User extends Model {
}

export default User;
```

恭喜你。您已经创建了自己的 API！很简单，对吧？现在您有了基本的 CRUD 请求。

但是，让我们添加更多的功能。让我们选择哪些字段将由用户填写。

```
class User extends Model {
  get fillable() {
    return {
      POST: ["email", "name"],
      PUT: ["name"],
    };
  }
}
```

我们不只是选择哪些字段是可填写的。我们还选择了哪些字段可以在哪些 HTTP 请求中填充。您的创建和更新请求现在是安全的。

让我们更进一步，为创建添加表单验证规则。

```
class User extends Model {
  get fillable() {
    return {
      POST: ["email", "name"],
      PUT: ["name"],
    };
  }

  get validations() {
    return {
      email: "required|email",
      name: "required|max:50",
    };
  }
}
```

就是这样。用户应该发送正确的数据。

但现在，是时候深入思考了。如果你有两个相关的模型，比如用户和帖子。让我们在模型定义中将它们绑定在一起。

```
class User extends Model {
  posts() {
    return this.hasMany("Post", "id", "user_id");
  }
}

class Post extends Model {
  user() {
    return this.belongsTo("User", "user_id", "id");
  }
}
```

定义之后，Axe API 将为您创建所有相关的路线。你能相信你会自动拥有以下路线吗？

*   `GET api/users`
*   `POST api/users`
*   `GET api/users/:id`
*   `PUT api/users/:id`
*   `DELETE api/users/:id`
*   `GET api/users/:usedId/posts`
*   `POST api/users/:usedId/posts`
*   `GET api/users/:usedId/posts/:id`
*   `PUT api/users/:usedId/posts/:id`
*   `DELETE api/users/:usedId/posts/:id`

# 业务逻辑

也许我能听到你说“是的，看起来不错，但是我们有不同的逻辑。例如，在用户创建中，我应该可以给密码加盐。”

但是你不知道的是 Axe API 为 HTTP 请求的每一层都提供了钩子。让我们像这样为模型创建一个`UserHooks.js`文件；

```
import bcrypt from "bcrypt";

const onBeforeInsert = async ({ formData }) => {
  // Genering salt
  formData.salt = bcrypt.genSaltSync(10);
  // Hashing the password
  formData.password = bcrypt.hashSync(formData.password, salt);
};

export { onBeforeInsert };
```

这个函数将在创建过程之前由 Axe API 触发。但不仅仅如此。Axe API 为您提供了以下所有钩子；

*   onBeforeInsert
*   onBeforeUpdateQuery
*   onBeforeUpdate
*   onbeforededeletequery
*   onbeforededelete
*   onBeforePaginate
*   onBeforeShow
*   onAfterInsert
*   onAfterUpdateQuery
*   onAfterUpdate
*   onAfterDeleteQuery
*   onAfterDelete
*   onAfterPaginate
*   午后秀

# 扩展查询功能

我之前说过，创建这样的框架有很多好处。比如说；扩展查询。一旦你定义了你的模型，它就可以被查询了。您可以发送如下查询:

```
GET /api/users
  ?q=[[{"name": "John"}],[{"$or.age.$gt": 18}, {"$and.id": 666 }]]
  &fields:id,name,surname
  &sort=surname,-name
  &with=posts{comments{id|content}}
  &page=2
  &per_page=25
```

通过这个查询，您可以询问以下内容:

*   如果`name`为“John”或`age`大于 18 且`id`为 666，则获取数据。
*   仅返回`id`、`name`和`surname`字段。
*   首先按`surname`排序(ASC)，其次按`name`排序(DESC)。
*   用相关的`comments`数据获取相关的`posts`数据。但是在`comments`对象中，只取`id`和`content`字段。
*   每页取 25 行。
*   取第二页。

无论何时创建模型，您都可以拥有这些扩展的查询功能。你不能说你不喜欢它！

# 下一步是什么？

嗯，还有很多功能我可以说说。但是我不打算创建另一个关于它的文档，因为我已经做了。请访问 [Axe API 文档](https://axe-api.github.io/)页面。你可能会发现关于这个项目的许多细节。

我正在征求每个有话要说的人的反馈。每当我想到这个项目，我都会因为它的潜力而兴奋不已。希望你也有同样的感受。

另外，请记住 **Axe API** 还没有准备好用于生产，它处于测试阶段。

你可以在 [GitHub](https://github.com/axe-api/axe-api) 上启动这个项目，并获得新闻通知。

> *感谢* [*封面图片*](https://pixabay.com/photos/greece-parthenon-temple-ruins-1594689/)