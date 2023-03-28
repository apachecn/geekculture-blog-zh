# 将木偶师作为插件集成到 Cypress 中。

> 原文：<https://medium.com/geekculture/integrate-puppeteer-as-a-plugin-in-cypress-1f0912d8e265?source=collection_archive---------3----------------------->

## 使用 Cypress 时需要通过不同的域登录吗？

## 请继续阅读，了解如何管理这些登录密码。

![](img/a97a7beb77c7213ac3a2d7cee5a8cf69.png)

Photo by [pine watt](https://unsplash.com/@pinewatt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/merge?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在我之前的一篇文章中，我曾讨论过与木偶师合作是多么容易。这一次，我需要木偶师来解决另一个网络自动化问题。

我和 [Cypress 一起工作来自动化](/technogise/adopting-cypress-again-97eddcdc4ea2)一个功能健全的套件，它不允许跨域导航——需要登录。

**问题定义:**

网站需要经过认证的用户，因此自动化功能测试也需要经过认证的用户。在我们的案例中，身份认证和授权由身份提供商(IdP)维护。

自然，我们想避免 GUI 登录，因为这超出了我们的测试范围。然而，我们尝试使用一个永久的 JWT 令牌——只能从自动化概要文件中接受——这次[没有成功](https://www.linkedin.com/pulse/containerised-user-journey-automation-using-cypress-karishma-sharma/)。

使用 IdP 意味着被测应用程序(AUT)将重定向到 IdP 的域，而不是 AUT 的域。我们知道这在赛普拉斯是不允许的。所以，我们决定使用一个木偶脚本作为插件。

> 他的脚本将使用 Chromium 登录到应用程序，并在 Cypress 测试使用的浏览器的本地存储中复制令牌。

***配置:***

*   木偶师的 npm 包被添加为一个依赖项。
*   这个文件有逻辑

```
cypress/plugins/puppeteer/login.js
```

*   为了让 Cypress 使用这个插件，配置了以下文件:

```
plugin/index.js
cypress/support/commands.ts
```

*   在*cypress/support/index . js*中导入命令文件(如上)

```
import ‘./commands.ts’;
```

*   我们在*cypress/support/index . d . ts .*中添加了 login 作为可链接的接口

带有使用示例的示例代码:

***管理机密:***

[Cypress](https://docs.cypress.io/guides/guides/environment-variables#Option-5-Plugins) 推荐的一种处理秘密的方式是使用插件，我选择了这个方式。下面是我如何实现它的。

你可能已经注意到了，我在 Cypress 上使用了 Typescript。你可以在这里阅读更多关于它的信息。