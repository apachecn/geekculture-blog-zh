# 用 Azure 函数和 C#为 Fitbit API 构建令牌刷新服务

> 原文：<https://medium.com/geekculture/building-a-token-refresh-service-for-the-fitbit-api-with-azure-functions-and-c-55027bf9d267?source=collection_archive---------10----------------------->

## 我们可以用一个简单的定时器触发函数生成一个刷新和访问令牌，以编程方式调用 Fitbit API。

![](img/6129ce262e2c76d5044d607e2fe973b9.png)

作为我个人发展的一部分，我正在 Azure 中建立自己的个人健康平台。我喜欢记录各种不同的健康指标，比如日常活动、食物摄入量和睡眠模式。为了收集这些数据，我使用了 Fitbit Ionic。

在过去，我习惯于下载一个每月的 CSV 文件，并对其进行一些基本的分析。这有点乏味，因为在我可以做任何事情之前，我必须手动清理数据。这花费了我不想浪费的时间，所以我想创建一个管道，只调用 Fitbit API 来检索我的数据，而不必自己手动清理文件。

使用 Fitbit API 相对简单。具有挑战性的部分可能是确保我们的更新令牌和访问令牌不会过期。默认情况下，这些将在 8 小时后过期。

幸运的是，我们可以使用 Azure 函数计时器触发器来请求新的访问令牌，并使用 Azure Key Vault 来安全地存储这些令牌。本文将向您展示如何使用 Azure 函数和 Key Vault 刷新 Fitbit API 的访问令牌。

虽然这个例子主要关注 Fitbit API，但是您可以将这个方法应用于其他 API 键。

**刷新令牌和访问令牌如何为 Fitbit API 工作**

Fitbit 使用 OAuth 2.0 对用户进行授权，并通过 Fitbit API 的认证。OAuth 2.0 框架需要我们的应用程序调用 Fitbit API 来获取访问令牌。这用于向 API 发出 HTTPS 请求。

Fitbit 支持授权码授权和隐式授权流。对于我的项目，我使用授权代码授权流。这是使用应用程序的客户端机密进行服务器到服务器通信的推荐选项。

使用授权码授权流，用户将同意与具有一些或所有范围的应用程序共享他们的 Fitbit 数据。当同意后，Fitbit 会给我们一个授权码，作为回叫 URL 的一部分。我们的应用程序将此代码交换一个访问和刷新令牌(我们将存储在密钥库中)。

默认情况下，这些令牌将过期。刷新令牌用于获得对新的访问和刷新令牌的访问。刷新令牌只能使用一次，因此我们需要在重新生成这些令牌时，将新的访问令牌和刷新令牌对存储在密钥库中。

如果您想了解如何在 Fitbit 中设置应用程序，获取客户端 id 和客户端密码，并生成授权码，以便能够生成刷新和访问令牌，我强烈推荐您观看下面的视频(本期视频为 Franchyze923 大喊！超级有帮助！)

**介绍我们的功能**

一旦你看了那个视频，你应该有一个客户端 id，客户端密码和一个 base64 编码的字符串，你的客户端 id 和密码用冒号连接(不知道我在说什么？再看一遍视频😉)

我们需要将它与我们之前生成的刷新令牌一起存储在我们的密钥库中。

在深入研究代码之前，让我们看看这个函数端到端地试图做什么。查看下图:

![](img/133cfef169803c5332946f50f0d22762.png)

每隔 6 小时，我们要启动一个刷新令牌的过程。我们可以增加刷新令牌的频率，但 6 小时对我来说已经足够了。当我们启动这个函数时，我们希望调用 Fitbit API 来刷新我们的令牌。

为此，我需要我现有的刷新令牌以及我的 Fitbit 凭证，我已经将它们作为秘密存储在 Key Vault 中。一旦我们有了这些，我们需要使用我们的令牌和凭证向 Fitbit API 发出请求，以生成新的令牌。

一旦我们有了令牌，我们希望将它们保存在 Key Vault 中，这样下次我们调用函数来再次刷新令牌时，我们将使用更新后的值。

让我们深入研究一下代码，让这一点更清楚。

**使用 HTTP 客户端拨打电话**

要调用 Fitbit API，我们可以使用 HttpClient 库！为了请求新的刷新令牌，我们需要进行以下 API 调用:

```
POST https://api.fitbit.com/oauth2/token
Authorization: Basic Y2xpZW50X2lkOmNsaWVudCBzZWNyZXQ=
Content-Type: application/x-www-form-urlencoded

client_id=22942C&grant_type=authorization_code&redirect_uri=https%3A%2F%2Fexample.com%2Ffitbit_auth&code=<somecode>
```

我们可以使用 HttpClient 库来构建 POST 请求，如下所示:

在本课程中，我们将做以下工作:

*   从密钥库中检索我们现有的刷新令牌和 Fitbit 凭据。(我使用密钥库 C# SDK 的包装器简化了这些调用)。
*   清除我们的请求头，定义我们希望向其发出 POST 请求的 URI，然后将查询参数定义为该 URI 的一部分。我们需要在这里使用我们的刷新令牌。
*   将请求定义为对我们的 URI 的 POST 请求。
*   将内容设置为空字符串，将标题和内容类型设置为使用“application/x-www-form-urlencoded ”,将我们的 AuthorizationHeaderValue 设置为使用基本身份验证并传入我们的 Fitbit 凭据。
*   然后，我们发出 POST 请求，确保它成功，并在返回之前将我们的响应反序列化为 RefreshTokenResponse 对象。

RefreshTokenResponse 是我创建的一个自定义类，因此我可以将它用作返回类型。其定义如下:

此类应该有助于将以下示例响应反序列化为强类型:

```
{
  "access_token": "eyJhbGciOiJIUzI1NiJ9.eyJleHAiOjE0MzAzNDM3MzUsInNjb3BlcyI6Indwcm8gd2xvYyB3bnV0IHdzbGUgd3NldCB3aHIgd3dlaSB3YWN0IHdzb2MiLCJzdWIiOiJBQkNERUYiLCJhdWQiOiJJSktMTU4iLCJpc3MiOiJGaXRiaXQiLCJ0eXAiOiJhY2Nlc3NfdG9rZW4iLCJpYXQiOjE0MzAzNDAxMzV9.z0VHrIEzjsBnjiNMBey6wtu26yHTnSWz_qlqoEpUlpc",
  "expires_in": 3600,
  "refresh_token": "c643a63c072f0f05478e9d18b991db80ef6061e4f8e6c822d83fed53e5fafdd7",
  "token_type": "Bearer",
  "user_id": "26FWFL"
}
```

现在我们有了新的访问令牌和刷新令牌，我们可以将它们保存到密钥库。

**在钥匙库里保存我们的秘密**

要保存我们的刷新和访问令牌，我们只需执行以下操作:

这里，我将传递我们的 RefreshTokenResponse 对象，该对象是我之前在 RefreshTokenService 类中获得的，然后使用之前的相同包装器将 RefreshToken 和 AccessToken 保存到密钥库中。

**端到端完成功能**

我已经将这两个组件分成了各自的类。经过一些整理，这是可能的，但是希望这个例子很容易理解。我们现在可以将这些服务注入到我们的功能中，如下所示:

**结论**

手动生成新的刷新令牌会很麻烦。幸好有了 Azure 函数定时器触发器，我们可以按计划生成令牌。通过将访问令牌存储在 Key Vault 中，我们可以在需要调用 Fitbit API 时检索令牌。

虽然这个例子主要关注 Fitbit API，但是希望您可以将这个例子应用到其他需要您在需要时刷新访问令牌的服务中。

如果您想查看这个示例的代码库，可以在这里查看:

[](https://github.com/willvelida/MyHealth.Auth.RefreshToken) [## willvelida/MyHealth。授权刷新令牌

### 为 willvelida/MyHealth 做贡献。Auth.RefreshToken 开发通过在 GitHub 上创建一个帐户。

github.com](https://github.com/willvelida/MyHealth.Auth.RefreshToken) 

希望你喜欢这篇文章。如果您有任何问题或意见，请随时在下面发表评论或在[推特](https://twitter.com/willvelida)上联系我。