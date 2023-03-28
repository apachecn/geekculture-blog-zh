# 五大基于 CDN 的云托管服务

> 原文：<https://medium.com/geekculture/top-5-cdn-based-cloud-hosting-services-31fef4f1341d?source=collection_archive---------17----------------------->

## 基于 CDN 的云托管服务及其支持的功能的精选列表。

![](img/58a4da9522d41f895052c7a780da5204.png)

Photo by [NASA](https://unsplash.com/@nasa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**注:**这是我根据自己对用例的体验得出的个人看法。本文中提到的每个服务提供者所提供的服务中可能还有其他特性，这在其他用例中可能是有利的。

## 我的使用案例

在试运行/生产环境中部署 ReactJS 应用程序(前端部分)。

如果你想知道为什么使用基于 CDN 的云托管服务，你可以阅读下面链接中的文章。

[](/geekculture/why-use-cloud-hosting-service-74b6ce061fcc) [## 为什么要使用基于 CDN 的云托管服务？

### 基于云 CDN 的虚拟主机服务的优势

medium.com](/geekculture/why-use-cloud-hosting-service-74b6ce061fcc) 

在比较本文中列出的 5 种云托管服务时，我考虑的总体特征是:

1.  **CDN 的规模和位置:**世界各地的边缘服务器(存在点)越多，为用户加载内容的延迟就越小。
2.  **支持的源代码库:**支持多个 git repo saas 提供商，例如:GitHub、Bitbucket 等。
3.  **易于部署:**支持以简单的方式部署应用程序。
4.  **更新预览支持:**每当您在 git repo 中更新您的前端设计或功能时，支持在将变更合并到主/生产代码分支之前预览变更。
5.  **创建/处理不同环境的简易性:**在所提供的服务中，是否有处理多种环境(如开发、试运行、生产)的简易方法。
6.  **轻松回滚:**如果任何部署出错都支持轻松回滚。比如一键回滚。
7.  **自由层限制:**自由层中提供的资源，如果有的话。

下面是云托管服务提供商的列表，

## 1. [AWS 放大器控制台](https://aws.amazon.com/amplify/hosting/)

1.  [AWS CDN](https://aws.amazon.com/cloudfront/features/?nc=sn&loc=2&whats-new-cloudfront.sort-by=item.additionalFields.postDateTime&whats-new-cloudfront.sort-order=desc) 在 90 个城市拥有 225 个以上的存在点(pop)。
2.  在控制台上默认支持 GitHub、Bitbucket、GitLab、AWS 代码提交。理论上，可以使用任何具有管道配置的 git repo，它可以运行 aws cli 命令。
3.  对于给定的分支，可以在每次提交 git repo 时部署该应用程序。支持可在 AWS Amplify web 控制台或 repo 中维护的管道配置文件 amplify.yml。通过 web 控制台或 CLI 进行一次性配置。
4.  它支持每个创建的拉请求的预览链接。
5.  支持为回购的不同分支创建不同的应用。比方说，一个应用程序用于生产部门，另一个用于试运行。每个都可以定制不同的配置，如环境变量/域等。
6.  当前不支持回滚。
7.  自由层限制在此[链接](https://aws.amazon.com/amplify/pricing/)中

## 资源:

1.  [使用 Amplify 控制台部署 ReactJS 应用程序](https://www.youtube.com/watch?v=DHLZAzdT44Y)
2.  [使用放大控制台](https://docs.aws.amazon.com/amplify/latest/userguide/pr-previews.html)拉请求预览

## 2.[GCP 主持的 Firebase】](https://firebase.google.com/products/hosting)

1.  谷歌云平台服务在 60 个大都市地区的 146 个网络边缘位置提供。
2.  没有通过 web 控制台的 git 配置。理论上，可以使用任何具有管道配置的 git repo，它可以运行 firebase cli 命令。例如: [Bitbucket](https://support.atlassian.com/bitbucket-cloud/docs/deploy-to-firebase/) ， [GitHub](https://www.youtube.com/watch?v=kLEp5tGDqcI) 等。
3.  使用需要安装的 [firebase-tools](https://firebase.google.com/docs/cli#mac-linux-npm) 包执行操作，并执行所需的命令。它使用 firebase.json。然而，自动化可以通过管道配置来处理，如 GitHub Actions/GCP 云构建。也可以使用基于 web 的 ide，如 [StackBlitx 和 Glitch](https://firebase.google.com/docs/hosting/use-cases#web_based_ide) 。
4.  预览支持是通过一个称为预览频道的功能提供的。它可以从任何分支创建而无需提交代码，默认的预览链接在 7 天后过期[。如果需要，也可以自定义到期时间。预览通道也可以自动为每个拉请求创建，如这里的](https://firebase.google.com/docs/hosting/manage-hosting-resources#preview-channel-expiration)[所示](https://firebase.google.com/docs/hosting/github-integration)
5.  不同的环境必须用从代表每个环境的不同分支创建的预览频道来管理。Firebase Hosting 还通过 Firebase 控制台和 Firebase CLI 提供工具来管理托管站点的[通道、发行版和版本](https://firebase.google.com/docs/hosting/manage-hosting-resources)。
6.  支持[回滚](https://firebase.google.com/docs/hosting/manage-hosting-resources#roll-back)。
7.  它总是有免费计划称为'火花'与限制给定的[在这里](https://firebase.google.com/pricing)

## 资源:

*   [在 firebase 主机上部署 ReactJS 应用](/swlh/how-to-deploy-a-react-app-with-firebase-hosting-98063c5bf425)
*   [在 firebase 主机上为 web 应用创建预览频道](https://www.youtube.com/watch?v=aIaDgD5R3qI)

## [3。Azure 静态 Web 应用](https://azure.microsoft.com/en-in/services/app-service/static/)

1.  Azure CDN 在 100 个大城市拥有大约 116 个 pop。
2.  支持 Github。
3.  开发和部署可以从 Web 控制台、CLI 或通过 Microsoft Visual Studio 代码(扩展)完成。默认情况下支持 GitHub 操作/Azure DevOps 管道。
4.  预览链接可用于每个拉取请求。
5.  可以为不同的分支创建单独的应用程序，每个分支代表一个环境。
6.  当前不支持回滚
7.  自由层限制如本链接中的[所示](https://azure.microsoft.com/en-in/pricing/details/app-service/static/)

## 资源:

*   [将 React 应用部署到 Azure 静态 web 应用](https://websitebeaver.com/deploy-create-react-app-to-azure-static-web-apps)
*   [在 Azure 静态 web 应用中创建拉请求预览](https://docs.microsoft.com/en-us/azure/static-web-apps/review-publish-pull-requests)

## [4。render.com](https://render.com/)

1.  根据网站提供全球安全 CDN 文件中未提供大小]。
2.  与 Github 或 GitLab 配合使用。
3.  使用在 web 控制台上完成的一次性构建配置，非常容易创建和运行初始部署。
4.  对于每个拉取请求，预览链接是自动创建的。
5.  可以为代表每个环境的分支创建单独的站点，或者如果更改很小，可以将拉式请求预览用作暂存版本。
6.  支持回滚。
7.  免费计划有限制，如这里给出的

## 资源:

*   【render.com 部署 react js App
*   【render.com 拉请求预览

## [5。网络生活](https://www.netlify.com/)

1.  使用其名为 [Netlify Edge](https://www.netlify.com/blog/2016/04/15/make-your-site-faster-with-netlifys-intelligent-cdn/) 的产品，该产品类似于 CDN，但具有更好的功能，旨在提供快速、可靠和安全的网络体验。企业级高性能边缘网络拥有 [70 多个全球接入点](https://www.netlify.com/enterprise/high-performance-edge/)。
2.  支持 GitLab、GitHub、Bitbucket
3.  在 web 控制台上或使用 Netlify CLI 完成简单的一次性构建配置，并提供发布目录路径，从该路径自动获取内容以进行部署。
4.  默认情况下，对于 GitHub 拉请求和 GitLab 合并请求，Netlify 构建[部署预览](https://docs.netlify.com/site-deploys/deploy-previews/)。
5.  允许多个[分支部署](https://docs.netlify.com/site-deploys/overview/#branch-deploy-controls)以及生产部署。
6.  支持回滚。
7.  它有免费的启动计划，限制在这里给出

## 资源:

*   [在 Netlify 中部署 ReactJS 应用](https://www.youtube.com/watch?v=JwWvD_fWJFY)
*   [将 react app 部署到 Netlify 的 3 种方式](https://blog.logrocket.com/3-ways-to-deploy-react-apps-to-netlify/)
*   [在网络上部署预览](https://docs.netlify.com/site-deploys/deploy-previews/)

# 结论

尽管上述所有服务提供商都提供了所需的大部分功能，但每家都有自己的使用方式。我希望这篇文章在您决定使用哪种服务之前对您有所帮助，并节省您的时间。如果您发现有任何遗漏或错误需要纠正，请发表评论，这将对本文的读者有所帮助。

**注:**本文提到的链接都不是附属链接。