# 从产品的角度，为高级开发人员开发 React

> 原文：<https://medium.com/geekculture/react-development-for-senior-developers-from-a-product-point-of-view-37cfcecf28de?source=collection_archive---------26----------------------->

![](img/cf9211e0b54b145606effb38f937c57a.png)

Source: Unsplash

现在，您已经开发了一个 React 应用程序，并且很高兴它运行得非常好。您已经实现了您的逻辑，您已经创建了足够数量的组件，您已经有了在后端运行的节点服务，数据绑定进行得非常好，并且在您的 UI 上呈现了许多很酷的视觉上吸引人的东西。

但是接下来呢？你以为你的发展结束了吗？

哈哈哈..答案是一个大大的否定。

从产品的角度考虑，老兄！！你真的认为这是你要给客户的最终产品吗？好吧，让我说，仍然有一些伟大的事迹留下来，在这篇文章中，我将简要地解释在你的代码创建之后你应该执行的那些重要的阶段。

> 请注意，我不会对每个阶段进行更深入的挖掘，因为已经有现成的官方文档可以为您提供关于如何实现它们的更详细的信息。我还在整篇文章中给出了一些**超链接**，以便引导你找到正确的来源。

所以，调整一下你的座位，喝杯咖啡，我们开始吧。

# **1。单元测试**

作为开发人员，测试我们创建的每个组件是我们的工作。React 带有 [Jest](https://jestjs.io/) 作为它的测试运行程序，特别是当你使用 [Create-React-App](https://create-react-app.dev/) 创建样板文件时。虽然 web 上有其他免费的测试框架，如 Mocha 和 Karma，但大多数 react 开发人员都喜欢使用 Jest，因为它的高性能和易用性。用 Jest 对你的组件进行单元测试有多种方法，最简单的是[快照测试](https://jestjs.io/docs/snapshot-testing)。

![](img/a635294b26a9e86f8032bb96cc1493a3.png)

一个典型的快照测试用例呈现一个 UI 组件，获取一个快照，然后将它与测试旁边存储的一个参考快照文件进行比较。如果两个快照不匹配，测试将失败:要么是*意外的*变更，要么是引用快照需要更新到 UI 组件的*新版本。*

虽然快照测试测试你的组件的渲染，如果你想测试一些特定的功能，你也可以通过[模仿它们](https://jestjs.io/docs/mock-functions)来完成。

现在，当谈到在我们的应用程序中使用 Redux 时，我们不应该忘记测试相关的[还原器](https://scotch.io/tutorials/testing-react-and-redux-apps-with-jest#toc-testing-your-reducers)和[动作](https://scotch.io/tutorials/testing-react-and-redux-apps-with-jest#toc-testing-your-action-creators-and-async-action-creators)。

# 2.静态代码分析

> 一位伟大的哲学家曾经说过，
> 
> “这不是你**什么**码，而是你**怎么**码”

任何人都可以写代码。但是，当你知道编码的好习惯时，你才是一个伟大的开发者。网上有很多工具可以用来审查你的代码，比如 Sonarqube，ESLint，JSlint 等等。，我个人更喜欢 [**ESLint**](https://eslint.org/) ，因为它附带了一个 VSCode 插件和一个 npm 包。它将帮助您在终端或控制台中以错误和警告的形式识别整个应用程序中的错误。

*   不使用的变量和导入应该从代码中删除，因为它们会消耗不必要的内存。
*   使用==的条件语句应该替换为 **===** 。如果需要，我们可以解析任何一边。
*   您可能经常在终端中看到一些警告，其中提到了*缺少 useEffect()的依赖关系。*我们应该在我们的函数组件中非常小心地使用 **useEffect()** 钩子，因为使用不当可能会导致内存泄漏问题。它带有一个副作用或依赖数组，对于不同类型的场景表现不同。关于**副作用**的更详细解释，请参考这篇[文章](https://dmitripavlutin.com/react-useeffect-explanation/#:~:text=1.%20useEffect%20%28%29%20is%20for%20side-effects%20A%20functional,output%20value%2C%20then%20these%20calculations%20are%20named%20side-effects.)。
*   避免使用内联样式属性，而是使用 [**CSS 模块**](https://create-react-app.dev/docs/adding-a-css-modules-stylesheet) 或 SASS 等预处理程序。
*   总是使用代码格式化程序来格式化你的代码。这将增强你和你的队友的可读性。您可以使用 [**更漂亮的**](https://prettier.io/docs/en/install.html) npm 包，并为其使用脚本命令，然后构建您的应用程序并将其推送到存储库。
*   如果您想在您的构建中对用户隐藏您的代码和源映射，您可以在您的构建脚本中包含 GENERATE_SOURCE_MAP=false。
    ***"构建"*:" GENERATE _ source map = false react-scripts build "**
*   使用 [ES6 功能](http://www.developer-cheatsheets.com/es6)如箭头功能等。，而不是传统的 JS 编码方式。

# 3.启用缓存机制

然而，这是可选的，但我仍然喜欢一些应用程序。我们可以通过使用浏览器的**本地存储**来减少 fetch 调用的数量。在这种情况下，对服务器的调用只会发生一次，除非我们重新加载页面。

# 4.启用安全性

![](img/8913761597fd662eda686507408e656f.png)

URL Source: Unsplash

这已经成为开发人员在构建 web 应用程序时必须注意的最重要的方面之一。如果不安全，整个应用程序可能会崩溃。

现在可能有各种类型的安全威胁。我已经提到了一些只能在客户端使用的技术。

## **安全来自** [**跨站脚本【XSS】**](https://owasp.org/www-community/attacks/xss/)**攻击** —

黑客可以以脚本标签的形式注入代码，从而完全控制用户浏览器中运行的应用程序。这主要针对表单字段，如输入、段落和文本区域。

**补救措施:**

*   只要您希望使用<输入>或< p >标签，就使用 React 的 **createElement()** API。
    `React.createElement('input', { placeholder: 'Enter a url', type: 'url', autoFocus: true }, 'default_url');`
*   使用**dangerouslySetInnerHTML**()—直接从 React 设置 HTML，而不是使用容易出错的`[innerHTML](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML).`
    `<div contentEditable='true' dangerouslySetInnerHTML={{ __html: "Hello" }}></div>`
*   您也可以通过使用一个名为 [**DOMPurify**](https://github.com/cure53/DOMPurify) 的库来净化您的输入。
*   [在这里](https://owasp.org/www-community/xss-filter-evasion-cheatsheet)你可以找到一系列的 XSS 攻击，可以用来测试你的应用中的 XSS 漏洞。

## 永远不要在本地存储中存储 JWT 代币—

在本地存储中存储 JWT 或 OAuth 令牌的风险非常高，因为有人可以很容易地打开开发工具来窃取令牌并滥用它们。

最好通过将令牌设置为 [HTTP Cookie](https://developer.mozilla.org/en-US/docs/Web/HTTP/Cookies) 来存储它们。或者，您也可以将令牌移动到 React 应用程序的状态。

## 永远不要在你的代码中包含客户秘密—

如果你在你的 Java 脚本代码中使用客户端秘密，这是不可原谅的罪过。您可以在您的环境文件中使用它们，因为这些文件通常不会被推送到 git 存储库中。

## 使用端到端加密—

为了防止黑客看到来自后端 API 的响应，我们可以在前端和后端都使用 [**crypto**](https://www.npmjs.com/package/crypto-js) 模块进行端到端的加密。举个例子，

*   从 React 端，我们可以首先加密 URL 端点。
*   在 **fetch()** API 中使用它来请求来自后端的响应。
*   另一方面，后端服务器接收加密的端点，并对其解密以获得确切的端点或路由。
*   后端服务器调用正确的路由来获得响应。
*   然后，后端服务器对响应进行加密，并将其发送回 React 服务器。
*   React 应用程序解密响应并相应地呈现视图。

## 使用内容安全策略(CSP)标题—

Content-Security-Policy 是现代浏览器用来增强文档(或网页)安全性的 HTTP 响应头的名称。Content-Security-Policy 头允许您限制资源(如 JavaScript、CSS 或几乎任何浏览器加载的内容)的方式。

虽然它主要用作 HTTP 响应头，但是您也可以通过 meta 标记来应用它。

CSP 最初是为了减少跨站点脚本(XSS)攻击的攻击面而设计的，该规范的后续版本还可以防范其他形式的攻击，如点击劫持。

我发现[这篇](/@nrshahri/csp-cra-324dd83fe5ff)文章对于设置 CSP 非常有用。

## 使用 Nexus 漏洞扫描器—

[Nexus Vulnerability Scanner](https://blog.sonatype.com/nexus-vulnerability-scanner-and-vulnerability-analysis#:~:text=What%20Is%20Nexus%20Vulnerability%20Scanner%3F%20Nexus%20Vulnerability%20Scanner,around%20100%2B%20open-source%20components%20and%20around%2020%2B%20vulnerabilities.)是一款扫描应用程序漏洞并给出分析报告的工具。

# 5.启用 gzip 压缩

[Gzip](https://www.gzip.org/) 是一种能够快速压缩和解压文件的数据压缩算法。压缩是节省网络带宽和提高应用程序速度的最简单方法。

![](img/08da32e00acc820d689c4f816d83c099.png)

我们可以使用 gzip 压缩大文件，图像，JS 文件等。

在收到来自前端服务器的响应时，后端服务器提取文件数据并搜索`Accept-Encoding`头以确定如何对应用程序进行编码。

如果服务器支持使用 gzip 压缩，资源将被压缩并通过网络发送。每个资源的压缩副本都添加了`Content-Encoding`头，指定资源使用 gzip 编码。

然后，浏览器在将内容呈现给用户之前，将其解压缩为原始的未压缩版本。

# 6.渐进式网络应用程序

![](img/b12e50d59a2f67ec802490a866fa272d.png)

然而，这是一个可选功能，取决于需求。一款**渐进式网络应用**是一款**网络**应用，为用户提供可与原生移动**应用**相媲美的体验。它的功能模仿了本地**应用**的功能。**渐进式 web 应用**的目标是提供类似**应用**的体验。

您可以通过这里提到的[步骤](https://create-react-app.dev/docs/making-a-progressive-web-app)将您的 React 应用程序转换为 PWA。

# 结论

正如科学天使所说，没有一台机器是完美和纯粹高效的。虽然类似的理论适用于 web 应用程序，但我们应该始终牢记，技术是与我们一起发展的。因此，我们必须始终确保我们始终能够找到开发和交付我们的应用程序的方法，这些应用程序在质量、健壮性、可伸缩性和性能方面都是最高的。

本文到此为止。我很快会添加更多的东西。:)