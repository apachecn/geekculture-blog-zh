# 为什么要使用基于 CDN 的云托管服务？

> 原文：<https://medium.com/geekculture/why-use-cloud-hosting-service-74b6ce061fcc?source=collection_archive---------51----------------------->

虚拟主机服务已经存在几十年了。随着尖端云技术的发展，许多服务提供商提出了具有高级功能的托管服务，这对开发人员/DevOps 人员来说是福音。

![](img/f346222ebdf846481055605d1b27f52d.png)

Photo by [Max Duzij](https://unsplash.com/@max_duz?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## TL；博士；医生

基于内容交付网络(CDN)的云托管服务非常有助于解决 web 应用程序(至少是客户端静态代码)的初始部署和持续集成/持续部署(CI/CD)中面临的大多数挑战，以及诸如拉式请求预览、安全性和监控资源等功能。它们主要通过将客户端代码部署到云中的 CDN，以无服务器模式公开客户端代码。

注意:这是我在我的用例中观察到的个人观点。

## 使用案例:

在本地完成基于 ReactJS 的 web 应用程序的开发后，我需要使用 easy CI/CD 将它部署到试运行/生产环境中。[对于此用例，您也可以考虑任何静态网站。]

## 此使用案例中面临的常见挑战有:

1.  **部署在哪里？**是裸机/虚拟机/容器/Kubernetes/云存储…？根据应用程序的类型、所需的可伸缩性、安全性和维护，部署基础架构的默认选项会有所不同。你必须在这些与其他参数如成本、资源和时间之间进行权衡。
2.  **如何部署？**作为容器镜像/包/简单 git 克隆…？根据您的需求，部署的方式可能会有所不同，从简单的 git 克隆到构建容器映像并在所需的平台/容器/Kubernetes 中部署它们。CI/CD 管道特性在这里对自动化很有帮助。
3.  **使用哪个代理来处理 SSL 证书？**虚拟机/云负载平衡器上的自托管代理服务器/…？要么我们必须在机器/虚拟机上维护自己的代理服务器，要么使用云提供商提供的托管负载平衡器服务，即使在那里我们也必须管理一些配置。
4.  **如何管理域名？为此，我们将应用程序指向主域或子域。但是，我们必须在完成域设置的地方管理配置(域提供商的 web 应用程序，如 go daddy/Google Cloud DNS/AWS Route 53 等)。**
5.  **如何获取 SSL 证书并进行管理？**这是另一项任务，如果使用自托管代理，您必须使用一些命令手动执行/设置 cron 如果使用任何云管理的负载平衡器，您必须在云负载平衡器上配置它。
6.  **如何在持续集成(CI)到应用程序之前审查变更？**我们是否将 **git 分支代码/容器映像**部署到某个环境中以供审查？

幸运的是，一些供应商提供的云托管服务非常有帮助，可以将上述所有工作减少到初始部署不到一个小时。这个领域有多个服务提供商。

我使用了 [AWS Amplify 控制台](https://aws.amazon.com/amplify/hosting/)作为我的用例【2】。但是大多数其他提供商也有这些功能。

让我们来看看 AWS Amplify 控制台如何以我最少的行动应对上述所有挑战，

1.  部署在哪里？AWS Amplify 在 AWS 内容交付网络(CDN)中托管代码，这是 AWS 的全球规模网络，用于更快的内容交付。**需要采取的行动:**无。
2.  如何部署？它有自己的管道配置，可以在 Amplify 控制台或代码报告中维护。有了这个配置文件，AWS Amplify 将知道最终的构建输出驻留在哪里，以便将其部署在 AWS CDN 中。**所需操作:**从 amplify 控制台连接 git repo，并在 Amplify 控制台或您的代码 repo 根文件夹[2]上配置 CI/CD 管道配置文件 amplify.yml。
3.  使用哪个代理？不需要设置代理，因为控制台中的域管理有自己的 SSL 配置。**需要采取的措施:**无。
4.  如何管理域名？如前所述，有一个管理域的部分指向应用程序。您可以配置域或子域，并在域注册商的设置中更新最低配置。**所需操作:**极少的域设置和配置[3]。
5.  如何获得 SSL 证书？完成域验证后，SSL 证书会自动生成。证书会自动更新。**需要采取的措施:**无。
6.  如何在持续集成(CI)到应用程序之前审查变更？当从一个特性分支创建一个新的拉请求时，分支中的更改将被部署，并使用 SSL 创建一个单独的 URL。此 URL 可用于在合并拉请求之前检查分支中的更改。**所需操作:**在控制台上启用拉请求预览功能，并在提交所需更改后从分支创建拉请求[4]。

+还有一些其他有用的功能。

*   **URL 的密码保护:**可以设置凭证来访问已部署应用的 URL。
*   **后端部署处理:**控制台还允许处理后端开发和内容管理。
*   **监控:**可以近乎实时地监控您的应用程序的托管相关指标，还可以创建警报，在指标超过您设置的阈值时发送通知。
*   **自由层:** AWS 为使用这些功能提供了一些自由层资源限制。它们足够好，可以免费开始使用，或者用作试运行环境。

关于 AWS Amplify 控制台 w.r.t .以上使用案例的参考链接:

*   [使用 Amplify 控制台初步部署 ReactJS 应用程序](https://www.youtube.com/watch?v=DHLZAzdT44Y)
*   [Amplify 控制台](https://www.youtube.com/watch?v=uaG2mMYLI68)中的自定义域名配置(注意:在视频中，他们使用 Route 53 作为域名注册商。它与任何域名注册后，域名所有权验证。)
*   [使用放大控制台拉取请求预览](https://docs.aws.amazon.com/amplify/latest/userguide/pr-previews.html)

## 优点概述，

1.  借助云 CDN 实现更快的内容交付。
2.  通过管道配置轻松实现 CI/CD 操作。
3.  内置 SSL 证书管理。
4.  轻松定制域配置。
5.  使用 PR 预览功能轻松查看变更。
6.  资源监控。
7.  仅为超出自由层限制的资源付费。

注意:这些优势在上述用例的范围内。如果范围扩大，可能会有额外的好处。如果你需要开发和管理后端 REST/GraphQL API，可以和 ReactJS 前端一起使用。

# 其他提供商:

以下是提供类似服务的其他平台列表，

*   [GCP 主持的 Firebase】](https://firebase.google.com/products/hosting?gclid=EAIaIQobChMIxsvJvef78QIVzgRyCh386AvTEAAYASAAEgL46vD_BwE&gclsrc=aw.ds)
*   [Azure 静态 Web 应用](https://azure.microsoft.com/en-in/services/app-service/static/)
*   [render.com](https://render.com/)
*   [网络寿命](https://www.netlify.com/)

感谢您的阅读。我希望这篇文章对云托管服务的优势有所启发。

你可以在下面的文章链接中找到排名前五的基于 CDN 的云托管服务，

[](/geekculture/top-5-cdn-based-cloud-hosting-services-31fef4f1341d) [## 五大基于 CDN 的云托管服务

### 基于 CDN 的云托管服务及其支持的功能的精选列表。

medium.com](/geekculture/top-5-cdn-based-cloud-hosting-services-31fef4f1341d) 

**参考文献:**

1.  [https://www.ibm.com/cloud/learn/what-is-cloud-hosting](https://www.ibm.com/cloud/learn/what-is-cloud-hosting)
2.  [使用 Amplify 控制台部署 ReactJS 应用程序](https://www.youtube.com/watch?v=DHLZAzdT44Y)
3.  [在 Amplify 控制台](https://www.youtube.com/watch?v=uaG2mMYLI68)中自定义域名配置(注意:在视频中，他们使用 Route 53 作为域名注册商。它与任何域名注册后，域名所有权验证。)
4.  [使用放大控制台拉请求预览](https://docs.aws.amazon.com/amplify/latest/userguide/pr-previews.html)