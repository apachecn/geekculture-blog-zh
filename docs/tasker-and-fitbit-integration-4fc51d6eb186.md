# Tasker 和 Fitbit 集成

> 原文：<https://medium.com/geekculture/tasker-and-fitbit-integration-4fc51d6eb186?source=collection_archive---------19----------------------->

在本文中，我将解释我如何使用 Tasker 在 Fitbit 上自动化一些操作。特别是，我的目标是创建一个任务，在特定条件下启用和禁用我的 Fitbit 的一些警报。

## 要求

*   Tasker 5.12.22
*   Fitbit
*   了解 Fitbit API 的一些基本开发技巧

所有的信息都可以在 Fitbit 提供的官方 [Web API](https://dev.fitbit.com/build/reference/web-api/) 站点上找到。

# 第一步。配置新的 Fitbit 应用程序

首先要做的是使用[我的应用](https://dev.fitbit.com/apps)门户创建一个新的应用。

登录后，我们只需点击**注册一个新的 app** 。

![](img/da6dcdf46a4af3d7e1f6db549d32790a.png)

并对其进行如下配置。

![](img/542bdc37b0d40cc0cbe416eec9a23271.png)

注意**重定向 URL** 很重要，因为它将用于获取访问令牌和**默认访问类型**，需要*读取&写入*选项来更改 Fitbit 设置或数据。

点击**保存**按钮，确认新应用的创建。在接下来的页面中，您需要复制 ClientId。

![](img/a458857a5fab4228f55033d41ad03efb.png)

# 第二步。了解授权流程

在[授权页面](https://dev.fitbit.com/build/reference/web-api/oauth2/#authorization-page)中，你可以看到你的应用应该如何请求用户授权访问他们的数据。

在我们的场景中，我们将使用*隐式授权流*来直接接收令牌。授权页面的 URL 是

> https://www.fitbit.com/oauth2/authorize?response _ type = token & client _ id =[您的客户端标识]& scope = settings & expires _ in = 31536000

URL 部分包括:

*   **https://www.fitbit.com/oauth2/authorize**:Fitbit 授权页面。
*   **response_type=token** :接收令牌。
*   **Client _ id =[your-Client-ID]**:在这里，您必须插入在创建应用程序后收到的客户 ID。
*   **scope=settings** :指定我们想从 Tasker 中访问哪种数据。你可以在这里阅读更多。
*   **expires_in=31536000** :令牌的有效期。31536000 是以秒为单位的一年。

如果您在设置您的客户端 id 后尝试该链接，您将被重定向到您在第一步中配置的**重定向 URI** ，并附加一些信息:

> [https://tasker . jooaapps . com/# access _ token =[very-long-token]&user _ id =[user-id]&scope = settings&token _ type = Bearer&expires _ in = 31536000](https://tasker.joaoapps.com/#access_token=eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIyM0I4WjgiLCJzdWIiOiI2NFM5QjQiLCJpc3MiOiJGaXRiaXQiLCJ0eXAiOiJhY2Nlc3NfdG9rZW4iLCJzY29wZXMiOiJ3c2V0IiwiZXhwIjoxNjU1MzYwNTY3LCJpYXQiOjE2MjM4MjQ1Njd9.g6xRfVlupQczjCsbipg7SLNgyIyq3U9EKvb8b_QYKj4&user_id=64S9B4&scope=settings&token_type=Bearer&expires_in=31536000)

我们将在任务中使用**访问令牌**参数。

# 第三步。熟悉 API

现在我们差不多准备好写任务了。我们首先需要理解我们将需要什么 API 调用以及如何调用它们。

你可以在 [Swagger](https://dev.fitbit.com/build/reference/web-api/explore/) 页面找到它们。

对于我们的场景，我们将使用 Auth 部分下的[内省调用](https://dev.fitbit.com/build/reference/web-api/explore/#/Auth/introspect)来检查令牌的有效性，并使用 [Devices](https://dev.fitbit.com/build/reference/web-api/explore/#/Devices) 部分下的调用来获取和更新警报。

## 试试邮差

我已经创建了一个 Postman 项目来测试我需要的所有调用。你可以用[这个文件](https://gist.github.com/pirasalbe/c90de06bb8dad339b85982746258a7e0)导入它们。

在测试它们之前，请确保在集合授权设置中配置您在上一步中收到的令牌。

![](img/61bf7f6e0ca9c846e23d6fc91ddb0707.png)

# 第四步。塔斯克

我们终于可以开始用 Tasker 写一些任务了。为了调用 Fitbit API，我们可以使用 **HTTP 请求**动作。

这是我做过的事情的清单。

## 检查并更新 Fitbit 令牌

我创建了一个配置文件，每天检查我的令牌的有效性。如果无效，它会向我发送一个通知，通知中有两个按钮，一个是再次授权应用程序，另一个是提交收到的新令牌。

[在这里](https://streamable.com/pjocw1)你可以看到一个简短的演示。当我点击第一个按钮时，它要求我用我的 Fitbit 帐户登录并授权应用程序。我以前做过这个，所以我没有再做就被重定向了。

## 选择要切换的警报

然后，我创建了一个任务，该任务执行调用以获取所有警报，并询问我想要自动管理哪个警报。这些警告被保存在一个数组变量中以备将来使用。

[在这里](https://streamable.com/1z4m0r)你可以看到一个简短的演示。

## 更新选定的警报

最后，我创建了一个任务来更新所有先前选择的警报，以便基于一个名为 *%Mode* 的变量来启用/禁用它们。

为了更新警报，我不得不使用**变量搜索替换**动作手动编码身体(可能不是最好的方法)。我硬编码了*工作日*和*循环*参数，因为我所有的闹钟都以相同的方式配置。

阅读[这个故事](https://pirasalbe.medium.com/tasker-and-fitbit-integration-tasks-overview-4d752c027a77?sk=0908951bc5aa826f404014508412a7b9)中对这些任务的详细解释。

# 第五步。导入项目

您可以使用下面的[链接](https://taskernet.com/shares/?user=AS35m8nzU8Z2OxB1WGqHbYHDLajdlq0DG%2B8k8f7L2XfQM3eHbHEAthA2xNvalA2%2FZaJvRZj2roE7ow%3D%3D&id=Project%3AFitbit)导入我创建的概要文件和任务。