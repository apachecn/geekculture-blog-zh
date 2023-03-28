# 用 Supabase 构建 Web 后端

> 原文：<https://medium.com/geekculture/building-backend-web-with-supabase-45eb8a97cdd7?source=collection_archive---------6----------------------->

![](img/ab0db77987880d36d3f1bc51fd74ee67.png)

Photo by [Christopher Robin Ebbinghaus](https://unsplash.com/@cebbbinghaus?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果你是一名前端 web 开发人员，在部署 web 应用程序到 GitHub pages、Vercel 等托管服务时，你肯定不会有任何困难。尤其是如果它只是一个静态的网站，作品集，或登陆页面。

但是说到上传后端部分，我有时候会很困惑该怎么做。我知道我可以使用 Postgres 将数据库上传到 Heroku，但另一个问题是，当我们在本地环境中使用 MySQL 开发数据库时，以及许多其他我不太熟悉的与后端相关的事情。

最近，我有一个个人项目，需要用户认证和一个数据库来存储一些数据。我知道我可以使用 Firebase 来实现它，但我认为这对个人项目来说太多了。此后不久，我看到 Twitter 上有人推荐其他人使用 *Supabase* 来存储一些简单的数据。我很好奇，最后，我为我的个人项目找到了一个解决方案。

[*Supabase*](https://supabase.io/)*是一个后端即平台(BaaS)服务，你可以在那里直接创建你的数据库，为你的网站进行用户认证等。他们提供三种主要产品，如数据库、存储和身份认证。我个人只用数据库和认证。*

*所以基本上我们只需要调用 Supabase 自动生成的 API 来做我们想要的任何操作，我们可以轻松地在 GitHub pages、Vercel 或任何其他虚拟主机中部署网站。*

*在我看来，他们的界面不错，尤其是在表格编辑器部分。我可以很快上手，按照我想要的方式安排数据库设计。它们还在我们的表中提供了外键关系等特性，这对我很有好处。*

*他们还为我们提供了自动生成的文档，告诉我们如何从表中获取数据以及其他操作，如编辑、更新、删除数据。从表中读取所有行非常简单，您可以*

```
**let* { data: blog, error } = *await* supabase.*from*('blogs').select('*')*
```

****免责声明:我在 React*** 中做了这个项目*

*我需要外键关系来链接帖子和评论，并维护“评论表”。因此，当某个帖子被删除时，该帖子附带的评论也会被删除。*

*我用 Supabase 做的另一件事是认证。他们提供了许多方法，如`signIn`、`signUp`、`session`等。真的很方便我们这些前端开发者去做。例如，当有人想要登录时，我们可以只编写这个简单的代码。*

```
*const { user, session, error } = await supabase.auth.signIn({email: 'emailFromUser',password: 'passwordFromUser',})*
```

# *结论*

*我终于能够创建后端，而不必上传我自己的后端代码和我需要做的所有其他配置。*