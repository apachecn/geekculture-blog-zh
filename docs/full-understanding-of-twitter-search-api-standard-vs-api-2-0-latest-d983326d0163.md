# 完全理解 Twitter 搜索 API-标准与 API 2.0(最新)

> 原文：<https://medium.com/geekculture/full-understanding-of-twitter-search-api-standard-vs-api-2-0-latest-d983326d0163?source=collection_archive---------2----------------------->

## 使用 Python 基于关键字在一个日期范围内提取推文

![](img/2010c9c97238323b8f72d794cedf9868.png)

Photo by [Alexander Shatov](https://unsplash.com/@alexbemore?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

嗨伙计们！🌞

在这篇文章中，我将详细探讨 Twitter API，我们将使用 python 库来实现它。而且，我还提到了哪一层有什么规格！那么，让我们开始吧🏃

# 目录:

1.  Twitter 搜索 API 简介
2.  费率限制和支持历史
3.  使用 tweepy 实现标准搜索 API
4.  使用 searchtweets 实现新的搜索 API v2.0
5.  其他来源和未来工作

## Twitter 搜索 API

为了使用过滤器从 Twitter 获取所有用户的推文，使用了 Twitter 搜索 API。Twitter 主要有 3 个版本

1.  标准版本
2.  高级版 1.1 版/企业版
3.  新的 API v2.0

## 费率限制

每天都有成千上万的开发者向 Twitter 开发者平台发出请求。为了帮助管理这些请求的绝对数量，对可以发出的请求的数量进行了限制。

*   **标准版本**:一个端点的速率限制为 900 个请求/15 分钟，这意味着每个用户在任何 15 分钟的时间间隔内最多允许 900 个请求，无论应用程序如何。它使用 OAuth1.0 身份验证方法。[ [阅读完整文档](https://developer.twitter.com/en/docs/twitter-api/v1/rate-limits)
*   **Premium v 1.1**:30 天/全存档的端点，以及*沙盒订阅* 的速率限制为 30 个请求/1 分钟。使用 *premium subscription* ，您每 1 分钟有 60 个请求。[ [完整文档](https://developer.twitter.com/en/docs/twitter-api/premium/rate-limits)
*   **API v 2.0:**tweet-lookup/Full-archive search**的一个端点的速率限制为每个应用 300 个请求/15 分钟，每个用户 900 个请求/15 分钟。[ [完整文档](https://developer.twitter.com/en/docs/twitter-api/rate-limits)**

## **支持历史**

**支持历史定义了你可以从 API 中获取多长时间的 tweets。**

*   **标准版:它有一个 7 天的搜索终点，这意味着你只能提取 7 天前的最新推文。**
*   ****Premium v1.1:** 它支持两个端点，一个 30 天端点和一个完全归档端点。**
*   ****API v2.0:** 最近和全存档搜索 REST 端点是搜索 Tweets 组的一部分。这些端点是对[标准版本 1.1 七天搜索](https://developer.twitter.com/en/docs/twitter-api/v1/tweets/search/overview)端点以及[高级版](https://developer.twitter.com/en/docs/twitter-api/premium/search-api/overview)和[企业版](https://developer.twitter.com/en/docs/twitter-api/enterprise/search-api/overview)全存档搜索产品的替代。**

> **那么，让我们编码吧**

## **使用[tweepy](https://docs.tweepy.org/en/v3.5.0/index.html)-python 实现标准搜索 API**

**让我们首先包含库，并粘贴您的 API 令牌和来自 Twitter 开发人员仪表板的访问令牌。**

**By [Author](/@harshkothari21)**

****注-1。**在上面的代码中，API 的“**wait _ on _ rate _ limit”**参数值得注意。由于 twitter 允许有限的请求，这个参数在继续运行搜索中起着重要作用。如果未指定该参数并且您访问了限制，则该过程终止。**

```
tweepy.API(auth, **wait_on_rate_limit=True**)
```

**By [Author](/@harshkothari21)**

****注意-2:** Tweepy.cursor()不再包含 since 参数，并且将找不到超过一周的日期的推文。**

****注-3:** 首先，标准的 Twitter API 不允许按时间搜索。一般来说，你能做的就是获取 tweets，然后在 Python 中查看它们的时间戳，但这是非常低效的。**

**[更多详情请参考[堆栈溢出](https://stackoverflow.com/questions/49731259/tweepy-get-tweets-between-two-dates)**

## **使用 searchtweets 实现新的搜索 API v2.0**

*   **将光标悬停在右上角的名称上时，从下拉列表中选择“Dev Environment”选项。**
*   **从您想要使用的 API 中单击“设置环境”。选择将在端点 URL 中使用的应用程序名称和标签名称，例如 developer。然后，完成设置。**

**让我们首先包含库并为 API 设置配置。你可以查看这篇文章了解更多细节。**

**By [Author](/@harshkothari21)**

**设置完成后，您可以简单地指定所需的文件管理器**

**By [Author](/@harshkothari21)**

****注-1:** results_per_call 参数的值为 100，这意味着每个请求将获取 100 条 tweets，这是免费轮胎的最大限制。**

****注意-2:** 如果您购买了套餐，每月费用为 100 美元，每次通话限制为 500 个请求。【[更多详情在此](https://developer.twitter.com/en/docs/twitter-api/premium/search-api/overview)**

****注-3:** 我在这里搜索特定日期范围内基于关键字‘AMZN’的推文。**

****注意-4:**eg 只允许特定的关键字(#hashtags，()grouping，links，' " keywords "-双引号中的关键字)。[ [更多详情](https://developer.twitter.com/en/docs/twitter-api/tweets/search/integrate/build-a-query) ]**

## **其他来源和未来工作**

**故障排除-如果您在 Api2.0 的认证中遇到任何问题— [ [Stackoverflow](https://stackoverflow.com/questions/55349475/twitter-premium-not-authorized) ]**

**故障排除 tweepy 的 Twitter 错误代码 429—[[stack overflow](https://stackoverflow.com/questions/41786569/twitter-error-code-429-with-tweepy)]**

**Twitter 的搜索 API 的完整列表给出了端点、支持历史和类别— [ [此处为](https://developer.twitter.com/en/docs/twitter-api/search-overview) ]**

**小项目——您可以计划对 covid 病例和疫苗进行情感分析。**

**👉感谢你阅读这个故事，❤.**