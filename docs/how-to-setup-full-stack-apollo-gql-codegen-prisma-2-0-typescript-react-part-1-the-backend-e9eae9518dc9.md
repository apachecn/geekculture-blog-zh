# 如何设置全栈 Apollo，GQL-CodeGen，Prisma 2.0，Typescript，React:第 1 部分—后端

> 原文：<https://medium.com/geekculture/how-to-setup-full-stack-apollo-gql-codegen-prisma-2-0-typescript-react-part-1-the-backend-e9eae9518dc9?source=collection_archive---------3----------------------->

![](img/8c032f9321f010c7c94b67066652e8df.png)

在我的上一篇文章中，我提到了在开发过程中使用栈来大大减少出错的机会。如果你想看看标题中的技术能带来的一些很棒的特性，请阅读下面的文章。

[](/geekculture/how-im-developing-my-full-stack-web-app-with-the-guards-up-62ae1a25cf3a) [## 我如何使用 Apollo 和 GraphQL 开发我的全栈 Web 应用程序

### 在本文中，我将分享 Apollo、Typescript、Prisma 2.0 和 GraphQL-codegen 是如何改变游戏的，当它…

medium.com](/geekculture/how-im-developing-my-full-stack-web-app-with-the-guards-up-62ae1a25cf3a) 

这将是一个循序渐进的教程，让项目具有文章设置中的工作流功能。

您需要了解 GraphQL 和 TypeScript 才能理解本教程。其他的技术我会带你去了解。

在这里找到这篇文章的代码[(分支“只是后端”是这篇文章特有的)](https://github.com/kieronjmckenna/apollo-prisma-typescript-tutorial.git)

# 数据库和 ORM (Prisma)

让我们把 Prisma(ORM)安装好。我将创建一个包含用户、帖子和赞的简单示例，仅仅是为了建立一些关系。

进入 VSCode 并添加 Prisma VSCode 扩展。它只是被命名为 Prisma 如果你有困难，看看 Prisma 网站。

Prisma 使用自己的语法。如果您不熟悉，请稍微阅读一下这些文档，但是这些文档足够直观，可以很快理解，我将尽力给出一个简要的概述。

```
mkdir backend && cd backend && npx prisma init
```

“prisma init”将为您做一些设置。一头钻进去。env，你会看到这个:

```
DATABASE_URL="postgresql://johndoe:randompassword@localhost:5432/mydb?schema=public"
```

如果您正在使用 Postgres，请根据您的需要更改这个 DB 连接字符串，因为我相信您以前已经见过了。如果你使用的是 MySQL 之类的数据库，请查阅 Prisma docs。

让我们用数据库创造一些奇迹吧。添加这个 prisma/schema.prisma，快速看一下。每个模型都是一个 DB 表。

如果您熟悉 GraphQL，您会注意到这个语法看起来很相似。实际上是这样，每个字段上都标记了一些额外的信息。

如果您愿意，可以跳过下一个代码块；只是为了炫耀 Prisma 的一个特点。我注意到两个主要特征。当你输入的时候，自动补全在最上面(我经常发送 ctrl+space)。亲自尝试下一个功能。

如果你把它放在你的 Prisma 文件中，并且有 Prisma 扩展名，它会告诉你犯了一个错误。作为检查，你没有把用户和文章之间的关系联系起来。这是很酷的部分；按保存。

然后，Prisma extension 将重写您的代码，使其看起来像这样，从而消除错误。

建立关系。为此向 Prisma 团队大声疾呼；印象非常深刻。

将 schema.prisma 改回如下:

从这里开始，我们将在数据库上创建表。

```
npx prisma migrate dev --preview-feature
```

这将应用更改。它还将安装相关的 Prisma 包，以便开始在我们的 Typescript 代码中查询 DB。

我还应该提到:如果你跑步:

```
npx prisma studio
```

在 http://localhost:5500 上，您的数据库将有一个令人惊叹的 GUI

无痛。是时候设置服务器了。

# 服务器(阿波罗服务器)

我将使用纱作为我的包装经理。如果您正在使用 npm，请适当地更改您的命令。

```
touch tsconfig.json && mkdir src && cd src && touch index.ts resolvers.ts typeDefs.ts
```

这就是我们需要设置的大部分内容。我们用“yarn init”启动一个节点项目，并在 src 文件夹中创建三个文件。我稍后将讨论这三个文件的作用。让我们安装一些依赖项。

```
yarn add apollo-server graphql && yarn add -D nodemon typescript ts-node @graphql-codegen/cli
```

它安装了我们开始工作所需的一切。

把这个放到你的 tsconfig.json 中。据我所知，这个配置相当标准。我的 tsconfig 设置的知识可以使用一些工作，所以让我知道这个项目是否有更好的设置。

并将该脚本添加到您的 package.json 中

# 让服务器运行起来

我先说 index.ts 这是我们将启动 Apollo 服务器和 GraphQL API 的地方。

对于这种配置，您可以不使用 runServer 函数。但是，如果您将来需要完成一些异步功能，这将使事情变得更容易。

代码非常简单；我们所做的就是启动 Apollo 服务器(它创建了 GraphQL API)并告诉它监听端口 4000。接下来我将讨论什么是解析器和类型定义。

```
const prisma = new PrismaClient()
```

根据我们创建变量“prisma ”,我们对所有模型执行 CRUD。例如:

```
prisma.post.findMany()
```

将返回数据库中的所有帖子。与其他一些节点 ORM 相比，我真的很喜欢 Prisma 的这种方法。没有从一些文件中导入文章，这些文件可能位于根目录的中间，有 5 个文件夹深。然后，我们将 Prisma 传递给 Apollo 服务器中的“上下文”。我们这样做是为了访问 Prisma，通过 GraphQL API 发送回数据。

# GraphQL 模式(typeDefs)

Apollo Server 让你指定关于你的 API 的一切。这意味着您需要知道每个查询将返回什么，以及它看起来像什么。您还可以指定每个查询的参数及其标量(类型)。毫不奇怪，这些都是用 GraphQL 语法完成的。

下一步是确保安装了 Apollo VSCode 扩展。

变量 gql 用于编写 GQL 代码以提供给 Apollo 服务器，并告诉 Apollo 扩展在哪里对 GQL 应用语法高亮显示。您会注意到 GQL 语法在 GitHub Gist 中没有突出显示。然而，由于 Apollo 扩展，我的 VSCode 看起来像这样:

![](img/dacc9bcfc41088653f2108595d2207f0.png)

扩展是必要的，因为我们正在用 Typescript 编写 GraphQL 代码。

深入研究代码——类型(User、Post、Like)是我们在 Prisma 中设置的镜像。

GraphQL 中有两种类型的请求:

*   查询—这些为您提供信息，但是查询不应该用于改变任何事情。仅用于查询。
*   突变——这些是用来改变信息的。

您会在模式中注意到它们。从现在开始，我将这些称为 **Q & M** 。

我们只需要实例化，所以将其添加到 src/resolvers.ts 中

```
export const resolvers = {}
```

你现在可以跑了

```
yarn start
```

您的 GraphQL 服务器将在 localhost:4000/graphql 上启动

# GraphQL 代码生成器

[GraphQL 代码生成器](https://www.graphql-code-generator.com/) (GQL-Codegen 为了方便在本文中作为 I 型)是由[公会](https://the-guild.dev/)发起的项目。

![](img/f64f8db17afc720c325a3a45ac394af2.png)

[https://www.graphql-code-generator.com/](https://www.graphql-code-generator.com/)

如图片所示，您使用 GraphQL 代码生成器根据您的模式生成代码。

如果您阅读了我的前一篇文章(如果您不熟悉这个主题，我建议您首先阅读，因为它比本教程更直观/更高级)，您会知道我们将使用 GQL-Codegen 来确保我们从 GraphQL API 向前端返回正确的数据。

对于上面的每个问答，我们需要一个函数来接收参数，然后返回数据。这就是阿波罗服务器所谓的解析器。

我们将使用 GQL-CodeGen 给 Typescript 关于这些参数和返回类型的信息，而不用显式地将接口/类型键入(GraphQL 代码**生成器**，明白了吧)。

奔跑

```
yarn graphql-codegen init
```

选择这些选项；

![](img/7cba32cc7f3d1d652653af1030f1972b.png)

*   按 enter 键输入模式的默认位置

![](img/1ce308a6e017249337201aa9fcb472a1.png)

*   保留默认输出位置。
*   键入“n”表示自省文件，除非您知道需要一个自省文件
*   输入默认配置文件名
*   package.json 命令的“生成”

这就是 GQL-CodeGen 设置。安装向导向我们的项目添加了一些依赖项。运行:

```
yarn install
```

接下来，确保您的服务器正在运行。如果没有，请使用以下命令重新启动它:

```
yarn start
```

GQL-Codegen 通过读取 GraphQL 端点(在我们的例子中是在 localhost:4000)来生成我们需要的代码。因此，既然我们已经运行了那个端点，我们可以告诉 GQL-Codegen 使用

```
yarn generate
```

这将创建文件 src/generated/graphql.ts，其中将包含生成的类型。

所以要完全理解 GQL-Codegen 将为我们做什么，我们需要理解解析器。

# 下决心者

正如我之前提到的，解析器只是一个返回查询或突变数据的函数。解析器还包括一些额外的功能，比如传递给请求的参数、上下文和其他超出本文范围的内容。

将这段代码添加到 src/resolvers.ts 中。您将看到我们在 typeDefs 中概述的每个 Q&M 都有一个返回数据的函数。查看解析器参数:“args”(第二个位置参数)是传递给请求的所有参数。我将在下面解释“上下文”(第三个位置论点)。如果您对此感到困惑，所有的查询和变化都嵌套在它们的同名对象中。

## 语境

如果你回想一下 index.ts:

还记得我说过当我们向 API 返回数据时，我们从上下文返回 Prisma 来访问它吗？我指的是分解器。

我们需要为 GQL-Codegen 提供我们的上下文类型，以获得 Prisma ORM 的自动完成功能。

在 src/目录中运行以下命令:

```
touch context.ts
```

并将这段代码添加到 src/context.ts 中。

所有代码所做的就是导出我们上下文的类型，其中包含 PrismaClient。现在我们需要告诉 GQL-Codegen 这是哪里。

转到 codegen.yaml，添加从“config”开始的所有内容。

与之前的 yaml 文件相比，我们所做的所有更改都在 config 下。如果您还记得，我们告诉 GQL-Codegen 在 src/generated/graphql.ts 下生成代码，因此相对于该文件，我们告诉它上下文类型位于哪个文件内，在文件位置之后，我们追加#Context，这是导出的接口的名称。

我们还将“useIndexSignature”设置为 true，这是由于 Apollo Server 的一些内部原因。

因为我们已经更改了配置，我们需要 GraphQL-Codegen 来生成一些代码运行:

```
yarn generate
```

## 把它放在一起

更改解析器，如下所示。我知道这看起来没什么，但是“Resolvers”拥有 GQL-Codegen 从我们的端点获取的所有信息(返回什么和每个函数的参数)，以及“context”类型。

以下是我第一篇文章中的 gif 图片，展示了我们在这里设置的内容将会对你有所帮助。它们与这个模式无关，但是它们显示了相同的自动完成和智能感知。

![](img/ccafd5342e5f7d561a5589837f0cfc08.png)![](img/3750eb877b51b89b05fa5c872a2b3a02.png)

如果这让你感到困惑(考虑到你已经读到这里，我希望不是这样)，读一读顶部链接的文章。

下面是一个将数据返回给我们刚刚制作的解析器的示例:

你需要读一下 Prisma docs 来完全理解这一点。但是请注意，我们可以通过上下文访问 Prisma。这样你就不用写了

```
const prisma = new PrismaClient()
```

在每个解析器的开始。

这是用 Prisma 2.0 执行 CRUD 的链接，这是我在这些解析器中所做的全部工作。

[](https://www.prisma.io/docs/concepts/components/prisma-client/crud) [## CRUD(参考)

### 本页描述如何使用您生成的 Prisma 客户端 API 执行 CRUD 操作。CRUD 是一个缩写词，意思是…

www.prisma.io](https://www.prisma.io/docs/concepts/components/prisma-client/crud) 

# 概述命令(以及何时运行它们)

```
npx prisma migrate dev --preview-feature
```

您已经对 Prisma 模式进行了**更改，并希望**将它们应用到您的数据库**。**

```
npx prisma generate
```

您已经对 Prisma 模式进行了**更改，并且您希望在您的**类型脚本代码**中处理这些更改(例如，您需要自动完成和智能感知来获取更改)。**

```
yarn start
```

启动 Apollo 服务器和您的 GraphQL 服务器/API。**注意:为了让下一个命令“yarn generate”工作，GraphQL API 必须运行。**

```
yarn generate
```

您已经对您的 **GraphQL 模式**进行了更改，并且您希望通过让 GQL-Codegen 完成它的工作(例如，更改解析器的返回类型或参数)来在您的**类型脚本代码**中使用它们

# 非常重要的一点

当您将“resolvers”类型应用于您的 Resolvers 对象时，如果任何解析程序的返回类型不正确，Typescript 将引发错误。这意味着你不能启动 Apollo Server，循环往复，意味着你不能让 GQLCodegen 生成代码；因为它需要一个 GraphQL 端点来生成代码。你看到了这个难题(我希望)。

简单的解决方案:从代码中删除“解析器”类型，启动服务器，再次生成代码，然后重新添加“解析器”。

更好的解决方案:看看阿波罗工作室。您可以将您的模式放在云中，这样您就可以进行更改并生成代码，而无需运行您的本地服务器。

[](https://www.apollographql.com/docs/studio/) [## 阿波罗工作室简介

### Apollo Studio 是一个云平台，可帮助您构建、验证和保护您组织的数据图。获得…

www.apollographql.com](https://www.apollographql.com/docs/studio/) 

# 结论

感谢阅读，我希望你觉得这是有用的。

这是本系列的第 2 部分，我们将在这里设置前端 React 项目:

[](https://kieron-mckenna.medium.com/how-to-setup-full-stack-apollo-gql-codegen-prisma-2-0-typescript-react-part-2-the-frontend-caf4e2e871de) [## 如何设置全栈 Apollo，GQL-CodeGen，Prisma 2.0，Typescript，React:第 2 部分—前端

### 如果你想知道为什么你应该遵循这个教程，请阅读我以前的文章，其中详细介绍了使用…

kieron-mckenna.medium.com](https://kieron-mckenna.medium.com/how-to-setup-full-stack-apollo-gql-codegen-prisma-2-0-typescript-react-part-2-the-frontend-caf4e2e871de) 

请在此找到该项目的代码:

[https://github . com/kieronjmckenna/Apollo-prisma-typescript-tutorial . git](https://github.com/kieronjmckenna/apollo-prisma-typescript-tutorial.git)