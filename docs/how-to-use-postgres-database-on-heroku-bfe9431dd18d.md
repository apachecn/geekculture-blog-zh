# 如何在 Heroku 上使用 Postgres 数据库？

> 原文：<https://medium.com/geekculture/how-to-use-postgres-database-on-heroku-bfe9431dd18d?source=collection_archive---------18----------------------->

![](img/958957b1e1ca934aece248c6a7992a0f.png)

1.  首先，在 [heroku](https://www.heroku.com/) 上创建一个新的应用

![](img/6662a708c633d8d01712822d90f696d6.png)

2.创建你的 heroku 应用程序后，你会看到这样的界面。

![](img/9df5b791f93489da3ee764c3f5c429fb.png)

> 假设:我假设你已经知道如何在 heroku 上部署后端服务器。

 [## 如何将 Node.js 应用程序部署到 Heroku

### 简介 Heroku 以使服务器配置变得简单和容易而闻名。我们可以更快地构建，而不必担心…

scotch.io](https://scotch.io/tutorials/how-to-deploy-a-node-js-app-to-heroku) 

3.转到**资源**选项卡。

![](img/fef7da54c2534118fe626a1868d86dc7.png)

4.在**附件**标题下，搜索 **Heroku Postgres。**

![](img/b6baf5d73b232e62d80adb5b4ca8f2a5.png)

5.选择 **Heroku Postgres** 选项&你会看到这样一个弹出窗口。

![](img/698eb585d7e6a4b4f3199cf6b801af2e.png)

6.继续按你的选择选择计划名称，但首先我使用了**业余爱好开发-自由**计划。

![](img/a9e615883fb9a142803a17b18b14f985.png)

7.选择计划后，点击**提交订单。**

![](img/fdd8fe5a00250739f72e75f70f6e5fa7.png)

8.现在，点击下面突出显示的 **Heroku Postgres** 。

![](img/663076ddecd1d99721c4ada909ecaa89.png)

9.您将被重定向到 heroku 提供的数据库管理网站。

![](img/99ea4f50c63e27e8b3b6e00825be7480.png)

10.选择**设置**选项卡。

![](img/d30c6e29e48884ef6154c5ab7a3a2c4b.png)

11.点击**查看凭证。**

![](img/a7f071f4aee19c478611151b10624264.png)

在 node js 应用程序(或任何其他应用程序)中使用这些数据库凭证&就是这样！

> `*console.log(‘Connected to Database!! Yippee.’)*`