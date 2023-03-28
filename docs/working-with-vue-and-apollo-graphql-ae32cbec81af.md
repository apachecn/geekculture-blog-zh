# 使用 Vue 和 Apollo GraphQL

> 原文：<https://medium.com/geekculture/working-with-vue-and-apollo-graphql-ae32cbec81af?source=collection_archive---------25----------------------->

![](img/50f10d48945132e42d91afde17eada94.png)

# 什么是 GraphQL

GraphQL 是传统 RESTFul API 获取数据的替代方法。许多公司开始将它作为他们技术堆栈的一部分，这已经成为行业趋势，包括[文章](https://www.article.com/)。它提供的好处包括指定您想要获取的响应，代码是文档，并允许您轻松地连接不同的服务来为前端提供数据。你可以从我的文章[迁移到 GraphQL](https://sdust.dev/content-posts-2020-07-19-moving-to-graph-ql/) 中了解更多关于它和我们迁移到 GraphQL 的计划。

# Vue 和 Apollo GraphQL

为了与 GraphQL 后端对话，通常我们会使用一个专门用来做这些事情的 GraphQL 客户端。将查询手写到 POST 或 GET 请求中并不是开发人员乐意做的事情，所以这些客户端可以让这变得更容易。有很多可用的客户端，我选择使用 [Apollo](https://www.apollographql.com/) 来为它的生态系统和社区服务。

为了这篇博文的目的，我假设 GraphQL 查询看起来像这样:

```
query Product($id: ID!) {
  product(id: $id) {
    title
    skuNo
    price
  }
}
```

Vue 3 的最终稳定版本即将发布，因此将在 Vue 3 中使用的代码示例。然而，如果您已经在 Vue 2 中使用了 Composition API，那么语法基本上是相同的。本文也不包括使用流行的库 [vue-apollo](https://github.com/vuejs/vue-apollo) 或@vue/apollo-composable，因为它们还没有正式使用 Vue 3。因此，我将直接使用 Apollo 客户端，这实际上是我们团队在文章中的做法。我个人喜欢这种方法，因为它可以更好地理解底层模块的工作方式，并且当我们引入新的层时，我们可以更好地理解哪个模块负责什么。

# 给我看看代码

让我们首先通过运行以下命令来安装 Apollo 客户端模块:

```
# if you are using yarn
yarn add @apollo/client

#if you are using npm
npm install @apollo/client
```

我首先创建一个 JavaScript 文件，它负责创建一个 Apollo 客户机。每当您需要在组件中发送一个 GraphQL 查询时，您将使用这个`createApolloClient`方法来获取一个 Apollo 客户机实例。如果您继续创建一个新的 Apollo 客户机实例，您将丢失以前积累的查询缓存。

```
import { ApolloClient, HttpLink } from '@apollo/client/core';
import { InMemoryCache } from '@apollo/client/cache';

let apolloClient = null;

export function createApolloClient() {
  if (apolloClient) {
    return apolloClient;
  }

  return apolloClient = new ApolloClient({
    link: new HttpLink({
      uri: "http://localhost:4001/graphql",
    }),
    cache: new InMemoryCache(),
  });
}
```

`link`属性基本上告诉 Apollo 客户机应该将查询或变化发送到哪里。通过利用`setContext`方法，您还可以将定制头作为查询的一部分，比如授权令牌。

```
import { ref } from "vue";
import { createApolloClient } from '../apollo.js';
import { gql } from '@apollo/client/core';

export default {
  name: "PageQuery",
  setup() {
    const apolloClient = createApolloClient();
    const result = ref(null);

    async function query() {
      try {
        const queryResult = await apolloClient.query({
          query: gql`
            query product($productId: ID!) {
              product(id: $productId) {
                title
                skuNo
              }
            }
          `,
          variables: {
            productId: 123,
          }
        });

        result.value = queryResult.data;
      } catch(error) {
        console.error(error);
      }
    }

    return {
      result,
      query,
    };
  }
}
</script>
```

Composition API 是 Vue 中当前选项 API 的一个替代，旨在将您的逻辑与您的 UI 分离，并让我们以更具功能性的方式编写代码。我的[多伦多 Vue 会议之旅](https://sdust.dev/content-posts-2019-12-02-vue-3-and-composition-api/)应该会帮助你了解更多。我在这里使用的`ref`是 Composition API 的一部分，基本上使它具有反应性。`gql`允许我们像在操场上一样编写 GraphQL 查询。我们引入了`createApolloClient`方法来创建一个客户端实例，并基本上实现了模板可以使用的查询功能。在这种情况下，我设置在点击事件时发送查询:

```
<template>
  <div class="page-query">
    <button @click="query">Send Query</button>
    <div>{{ result }}</div>
  </div>
</template>
```

响应返回并保存到`result`变量，Vue 更新模板。

# 笔记

当我们创建 Apollo 客户机时，我们给了它一个 InMemoryCache 实例，这意味着默认情况下，它将开始缓存所有以前请求的响应，因此如果 UI 请求相同的查询，它不需要发送另一个网络请求并给你即时反馈。然而，如果你每次都需要刷新响应，那么你可以传递一个值给`fetch-policy`。你可以在[阿波罗文档](https://www.apollographql.com/docs/react/api/core/ApolloClient/)中读到更多关于其他可接受值的信息。请注意，官方的 Apollo 客户端使用 React 方式做事情，但在本文中我使用的是底层的 Apollo 客户端。

```
await apolloClient.query({
  query: gql`
    query product($productId: ID!) {
      product(id: $productId) {
        title
        skuNo
      }
    }
  `,
  variables: {
    productId: 123,
  },
  fetchPolicy: 'network-only'
});
```

如果你觉得有用，请分享给其他人，让他们知道如何将 GraphQL 整合到他们的 Vue 应用中。

*原发布于*[*https://sdust . dev*](https://sdust.dev/content-posts-2020-09-11-working-with-vue-and-apollo-graph-ql/)*。*