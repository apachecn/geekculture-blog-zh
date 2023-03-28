# 如何在不编写代码的情况下将您的应用程序与 GraphQL server 集成。

> 原文：<https://medium.com/geekculture/how-to-integrate-your-app-with-graphql-server-without-writing-code-fb665a2537f2?source=collection_archive---------5----------------------->

GraphQL 现在越来越受欢迎，这并不奇怪。GraphQL 比 REST API 灵活得多，您可以在一个请求中从后端接收所有需要的数据。今天我将展示如何不用写一行代码就为 GraphQL 服务器创建一个客户端，好吧，好吧，不完全不用写代码，只用几行代码:)

![](img/309d5e2554767aa2fb43e02587cdacad.png)

# 代码生成

这是一个相当古老且不言自明的概念。您将一些模式定义传递给代码生成器，它会在某种编程语言上创建现成的代码。例如:您可以从数据库模式生成 ORM 实体代码或迁移代码。这个概念在 Golang 中变得非常流行，因为 Golang 不支持泛型。但是 GraphQL 呢？

# GraphQL 代码生成器

来自[公会](https://the-guild.dev/) 的家伙们创造了一个很棒的工具——[GraphQL 代码生成器](https://www.graphql-code-generator.com/)(我不是在推广公会组织，我不知道他们是谁)，这个工具从 graph QL 模式生成代码。

![](img/bb279fae35cd4356d9e6f373479770c1.png)

GraphQL 代码生成器支持[类型脚本](https://www.graphql-code-generator.com/docs/plugins/typescript)、[流程](https://www.graphql-code-generator.com/docs/plugins/flow)、[原因](https://www.graphql-code-generator.com/docs/plugins/reason-client)、 [Java](https://www.graphql-code-generator.com/docs/plugins/java) 、 [Kotlin](https://www.graphql-code-generator.com/docs/plugins/kotlin) 和 [C#](https://www.graphql-code-generator.com/docs/plugins/c-sharp) 。这意味着你可以使用它进行 web、后端、iOS 和 Android 开发。让我们试着为网络应用创造一些东西。如果我们打开 docs 中的 [TypeScript 部分](https://www.graphql-code-generator.com/docs/plugins/typescript)，我们可以看到有几个不同框架的插件，比如 React、Vue、Angular 和 Svelte。但是我更感兴趣的是不与某些框架紧密耦合的通用解决方案。最受欢迎的 GraphQL 客户端是`[apollo-client](https://www.npmjs.com/package/@apollo/client)`和`[graphql-request](https://www.npmjs.com/package/graphql-request)`，根据 NPM 的数据，它们每周的下载量几乎相等，你可以在前端和后端下载。我更喜欢`graphql-request`，因为它非常简单、轻便。而且不用担心缓存失效，因为`graphql-request`不支持任何缓存。

# 类型生成

让我们安装所有必需的依赖项:

```
yarn add graphql graphql-tag
yarn add --dev @graphql-codegen/cli @graphql-codegen/typescript
```

`graphql`，`graphql-tag`将用于在运行时发出 GraphQL 请求，`@graphql-codegen`包仅用于开发目的。

为了测试，我们可以使用一些公共的 GraphQL 服务器，例如[https://api.spacex.land/graphql/](https://api.spacex.land/graphql/)。让我们用 GraphQL 代码生成器配置创建`codegen.yaml`:

并运行它:

```
yarn graphql-codegen
```

成功执行后，GraphQL 代码生成器会创建一个巨大的 TypeScript 文件，其中包含所有已定义模式类型的类型定义。它有 1321 行，所以我在这里只添加了一部分:

`Maybe`、`Exact`、`MakeOptional`和`MakeMaybe`是实用程序类型，它们将在生成的代码中广泛使用。下一个类型是`Scalars`，实际上它不作为类型使用，它是一个以 GraphQL 标量类型作为键，以 TypeScript 类型作为值的映射。如你所见，有些错误:`Date`、`ObjectID`、`timestamptz`、`uuid`映射为`any`，但实际上是字符串。让我们通过添加类型映射来修复它:

再次运行`yarn codegen`后`Scalars`变为:

在`Scalars`之后是型号类型。好了，我们有类型了，接下来呢？

# SDK 生成

现在，我们将尝试生成一些用于访问 GraphQL API 服务器的代码。首先，我们需要安装所需的插件:

```
yarn add --dev [@graphql](http://twitter.com/graphql)-codegen/typescript-operations [@graphql](http://twitter.com/graphql)-codegen/typescript-graphql-request
```

向`codegen.yaml`添加新插件:

并运行`yarn graphql-codegen`，在脚本执行后这段代码被添加到`graphql.sdk.ts`的末尾:

`getSdk`函数返回空对象，为了用有用的东西填充它，我们需要用 GraphQL 查询创建一个文件。

重要:GraphQL 查询必须有名称，否则 GraphQL 代码生成器会忽略它。

并重新配置`codegen.yaml`来扫描 GraphQL 查询文件:

在运行`yarn graphql-codegen`之后，我们得到这个:

GraphQL 代码生成器已经自动为 GraphQL 查询变量和响应创建了类型，并将其包装到方法`TestQuery`。最后一步，将 SDK 挂载到`graphql-request`实例。先别忘了`graphql-request`:

```
yarn add graphql-request
```

就这样，SDK 就可以用了！要添加新的 GraphQL 查询，只需添加一个文件并重新运行`yarn graphql-codegen`。正如我所承诺的，我们只写了 4 行代码，其他部分是配置和 GraphQL 查询。

# 奖金

![](img/9286c7afde1e64a17ba1474331509138.png)

生成的代码可能不符合你的 linter 规则，你可以将它标记为忽略，或者你可以自动将`/* eslint-disable */`添加到生成文件的顶部。通过安装特殊插件:

```
yarn add --dev @graphql-codegen/add
```

并补充到`codegen.yaml`:

为了简化生成的代码，你可以去掉`__typename`，当然如果你不使用它的话。

GraphQL 代码生成器可以在手表模式下运行，方法是将`watch: true`添加到`codegen.yaml`:

此外，您可以在开发服务器上同时运行 GraphQL 代码生成器。创建 React 应用程序的示例:

```
"start": "yarn codegen & react-scripts start"
```

# 结论

开发人员应该自动化他们的开发过程。为外部服务编写类型是一个耗费精力和时间的过程。节省你的时间！配置自动代码生成，并使用它，直到你被解雇。

*今天就到这里吧！下次见！Servus！*