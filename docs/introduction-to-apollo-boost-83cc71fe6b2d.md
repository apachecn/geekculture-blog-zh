# 阿波罗助推简介

> 原文：<https://medium.com/geekculture/introduction-to-apollo-boost-83cc71fe6b2d?source=collection_archive---------9----------------------->

![](img/d6e185eb81bf5f89ac0ce67b69c90802.png)

# 阿波罗助推简介

在本文中，我们将讨论阿波罗助推。这是一种零配置方法，也是最容易开始使用 Apollo 客户端的方法。它的维护是可行的，证明了一些项目活动。npm 包 apollo-boost 获得了总共 286，731 次每周下载。

阿波罗-增强吸引力被归类为有效的项目。npm 包 apollo-boost 被仔细检查，以查找已知的责任和丢失的许可证。没有这样的问题产生。因此，该包装被认为可以安全使用。

# 描述

Apollo Boost 是快速开始使用 [Apollo](https://www.technologiesinindustry4.com/2021/08/apollo-graphql-platform.html) 客户端的一种过度方式。然而，它并不关心开箱即用的一些激进特性。换出 Apollo 缓存，或者增强当前 Apollo 到网络堆栈的链接，如果我们想使用订阅的话。现在还不包括，我们必须手动设置 Apollo 客户端。

Apollo Boost 包含了一些我们认为对 [Apollo](https://www.technologiesinindustry4.com/2021/08/apollo-graphql-platform.html) 客户端来说很重要的包。下面是盒子里的东西:

*   阿波罗-客户:所有的魔法都在这里发生
*   apollo-cache-in-memory:这是推荐的缓存
*   这是用于远程数据获取的
*   阿波罗-链接-错误:这是错误控制
*   graphql-tag:它为查询和变异导出 gql 函数

关于 [Apollo](https://www.technologiesinindustry4.com/2021/08/apollo-graphql-platform.html) Boost 的压倒性的事情是，我们不需要自己设置这些。如果我们想使用这些功能，只需陈述几个选项，其余的我们会小心处理。

# 装置

*   最初，安装 apollo-boost，项目中没有 graphql 和 react-apollo，然后类似地连接它们。

```
npm i apollo-boost graphql react-apollo –S
```

*   然后，创建客户端。
*   一旦创建了客户端，通过将它传递给从 react-apollo 传输来的 ApolloProvider，将它挂接到应用程序。

```
import React from 'react';import { render } from 'react-dom';import ApolloClient from 'apollo-boost';import { ApolloProvider } from 'react-apollo';// Pass your GraphQL endpoint to uriconst client = new ApolloClient({ uri: 'https://nx9zvp49q7.lp.gql.zone/graphql' });const ApolloApp = AppComponent => ( <ApolloProvider client={client}> <AppComponent /> </ApolloProvider>);render(ApolloApp(App), document.getElementById('root'));
```

[ApolloClient](https://www.technologiesinindustry4.com/2021/08/apollo-graphql-platform.html) 目前连接到 app。现在，创建< App / >组件，并创建第一个查询:

```
**import** React **from** 'react';**import** { gql } **from** 'apollo-boost';**import** { Query } **from** 'react-apollo';const GET_MOVIES **=** gql` query { movie(id: 1) { id title } }const App **=** () => ( **<**Query query**=**{GET_MOVIES}**>** {({ loading, error, data }) => { **if** (loading) **return** **<**div**>**Loading**...</**div**>**; **if** (error) **return** **<**div**>**Error **:**(**</**div**>**; **return** ( **<**Movie title**=**{data.movie.title} **/>** ) }} **</**Query**>**)
```

# 基本迁移

如果我们在 [Apollo Boost](https://www.technologiesinindustry4.com/2021/08/apollo-graphql-platform.html) 上不使用任何配置选项，迁移应该相对简单。我们需要改变的只是初始化 ApolloClient 的文件。

# 更早的

Apollo Boost 的客户端初始化如下所示:

```
import ApolloClient from "apollo-boost";const client = new ApolloClient({ uri: "https://w5xlvm3vzz.lp.gql.zone/graphql"});
```

# 后来

首先，我们需要安装一些包来制作一个与 [Apollo Boost](https://www.technologiesinindustry4.com/2021/08/apollo-graphql-platform.html) 一样默认的基本客户端。

```
npm install apollo-client apollo-cache-inmemory apollo-link-http apollo-link-error apollo-link graphql-tag --save
```

我们需要手动将缓存和链接附加到客户端，以完成该过程。

```
import { ApolloClient } from 'apollo-client';import { InMemoryCache } from 'apollo-cache-inmemory';import { HttpLink } from 'apollo-link-http';import { onError } from 'apollo-link-error';import { ApolloLink } from 'apollo-link';const client = new ApolloClient({ link: ApolloLink.from([ onError(({ graphQLErrors, networkError }) => { if (graphQLErrors) graphQLErrors.forEach(({ message, locations, path }) => console.log( `[GraphQL error]: Message: ${message}, Location: ${locations}, Path: ${path}`, ), ); if (networkError) console.log(`[Network error]: ${networkError}`); }), new HttpLink({ uri: 'https://w5xlvm3vzz.lp.gql.zone/graphql', credentials: 'same-origin' }) ]), cache: new InMemoryCache()});
```

更多详情请访问:[https://www . technologiesinindustry 4 . com/2021/11/introduction-to-Apollo-boost . html](https://www.technologiesinindustry4.com/2021/11/introduction-to-apollo-boost.html)