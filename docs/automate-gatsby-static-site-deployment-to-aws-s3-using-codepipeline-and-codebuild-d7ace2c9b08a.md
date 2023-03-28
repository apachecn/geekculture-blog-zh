# 使用 CodePipeline 和 CodeBuild 自动将 Gatsby 静态站点部署到 AWS S3

> 原文：<https://medium.com/geekculture/automate-gatsby-static-site-deployment-to-aws-s3-using-codepipeline-and-codebuild-d7ace2c9b08a?source=collection_archive---------14----------------------->

![](img/2e156b1ee5fd65ef52d4ae0db02a7577.png)

Photo by [James Harrison](https://unsplash.com/@jstrippa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 简介+先决条件

Gatsby 是一个非常棒的前端框架，可以快速开发和部署静态网站。有一个由漂亮的模板和插件组成的丰富的生态系统，适用于各种用例，如博客和作品集网站。

有几个服务，你可以很容易地部署便宜的静态网站，但是在这篇文章中，我们将讨论如何使用 CodePipeline、CodeBuild 和 S3 自动部署到 AWS。我们将:

1.  用我们的代码将 CodePipeline 管道连接到 GitHub 库。
2.  创建一个 S3 存储桶，并将其配置为托管一个面向公众的网站。
3.  创建一个 CodeBuild 项目，该项目将构建我们的代码并将其部署到我们的 S3 存储桶中。

*本文内容针对 AWS 初学者/中间用户。*

# 先决条件

1.  包含您要部署的网站的 GitHub 存储库。如果你需要一个例子，你可以试着用[这个博客主题](https://github.com/LekoArts/gatsby-themes/tree/master/themes/gatsby-theme-minimal-blog)来开始。自述文件中的说明将告诉您如何开始使用它。
2.  AWS 账户。

# S3 设置

使用 S3 桶进行网站托管是一个常见的用例，其配置相对简单。

1.  转到 S3，创建一个新的存储桶。
2.  输入存储桶名称。如果您将使用自定义域，您应该将存储桶命名为(example.com)。
3.  取消选中“阻止*所有*公共访问”。我们需要互联网上的人们能够访问我们的网站资产。
4.  点击**创建存储桶**

现在已经创建了存储桶，导航到它:

1.  进入**属性**标签，编辑“静态网站托管”
2.  启用静态网站托管，为**托管类型**选择“托管静态网站”。
3.  对于**索引文档**和**错误文档**，分别输入“index.html”和“404.html”，或者您的站点构建的任何 html 文件作为索引和错误文档。
4.  点击**保存更改**
5.  最后，转到**权限**选项卡，将这个 JSON 输入到**存储桶策略**中:

```
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "PublicReadForGetBucketObjects",
      "Effect": "Allow",
      "Principal": "*",
      "Action": "s3:GetObject",
      "Resource": "arn:aws:s3:::<BUCKET_NAME>/*"
    }
  ]
}
```

你的 S3 桶应该准备好了。

# 代码管道设置

现在我们将创建一个代码管道，它将在我们每次推送至 GitHub 存储库时执行。导航到代码管道控制台，点击**创建管道**。在**步骤 1** 中，选择一个名称，为管道选择*新服务角色*，进入下一步。

## GitHub 源

在**步骤 2** 中，我们将连接到我们的 GitHub 帐户，并将我们的库作为源代码阶段添加到 CodePipeline 中。在**源提供商**下，选择 GitHub(版本 2)。这将打开以下更多选项:

1.  对于连接，选择一个现有的连接，或者，如果您以前从未将您的 AWS 帐户连接到 GitHub，请单击**连接到 GitHub** 。这将提示您登录 GitHub，并确认您想要创建一个连接。
2.  创建连接后，选择您想要使用的**库名**和**分支名**。
3.  保留所有其他选项的默认值，因为此管道不需要它们。转到下一步。

## 代码构建

在**步骤 3** 中，选择 CodeBuild 作为构建提供者，然后点击 **Create Project** 打开一个新窗口来创建一个新的 CodeBuild 项目。输入名称，暂时保留默认选项。我们将在下一节讨论代码构建步骤的细节。

**步骤 4** 是代码管道中的“添加部署阶段”步骤。因为我们将在构建步骤中将静态站点构建文件复制到 S3，所以我们可以跳过这个阶段，继续创建管道。

# 代码构建设置

导航到 CodeBuild 控制台的 **Build Projects** 部分，您应该看到在设置管道时创建的项目的名称。

选择构建项目并转到**编辑**，然后转到**构建规范**。

buildspec.yml 文件包含在存储库中是很常见的，因此它也可以被版本控制，但是对于这个项目，我们将把我们的构建命令直接插入 AWS 控制台。

确保用您的 S3 桶的实际名称替换`BUCKET_NAME`。

## buildspec.yml

```
version: 0.2

phases:
  install:
    commands:
      - "touch .npmignore"
      - "npm install -g gatsby"
  pre_build:
    commands:
      - "npm install"
  build:
    commands:
      - "npm run build"

  post_build:
    commands:
      - 'aws s3 sync "public/" "s3://<BUCKET_NAME>" --delete --acl "public-read"'

artifacts:
  base-directory: public
  files:
    - "**/*"
  discard-paths: yes
```

这是一个简单的构建规范。它安装我们的依赖项来构建一个 Gatsby 站点，然后运行将在 package.json 中定义的`npm run build`。在 *post_build* 阶段，我们使用 AWS CLI 将静态站点资产(我们希望在 public/ directory 中)复制到我们用来托管站点的 S3 桶中。

## 附加策略以允许写入 S3 存储桶

为了给予我们的构建项目访问我们的 S3 存储桶的许可，我们需要将一个适当的 S3 访问策略附加到构建项目的服务角色。为此:

1.  导航到 **Build details** 选项卡，找到项目的服务角色。服务角色列在**环境**下。单击服务角色链接打开 IAM。
2.  将`AmazonS3FullAccess`策略附加到服务角色。请注意，在生产中不建议这样做，因为该策略允许访问所有存储桶和所有操作。您应该创建一个策略，仅定义对特定 S3 存储桶的写访问权限。

完成后，尝试执行管道，看看它是否运行完成。如果一切顺利，您应该能够在 S3 桶中看到您的构建文件，并且应该通过转到桶的 URL 来看到您的网站。

# 奖励:CloudFront 分发+ AWS 证书管理器+自定义域

我们应该做一些事情来使我们的网站更好地为生产做准备。

S3 的网址并不完全漂亮，我们应该使用自定义域名。我们还希望能够使用 SSL/TLS 访问我们的网站，因此需要一个证书。最后，如果我们希望降低访客体验的延迟，我们应该将我们的资产托管在 CDN 中。以下部分将概述如何实现上述所有目标。

## 用于 SSL/TLS +自定义域设置的 AWS 证书管理器

如果您有自定义域，Amazon 可以使用 AWS 证书管理器为其提供 SSL/TLS 证书。在证书管理器控制台中为您的域申请证书，并按照步骤验证您是否拥有您的域。

验证完成后，如果你是*而不是*要创建一个 CloudFront 发行版，你可以添加一个指向 S3 bucket URL 的 CNAME 记录，这样你就可以使用 HTTPS 访问你的网站。

## CloudFront 分发设置

使用 S3 托管时，设置 CloudFront CDN 发行版很简单。

1.  转到 CloudFront 控制台，点击**创建发行版**
2.  选择您的 S3 时段作为起始域，并输入起始域的名称。
3.  在**默认缓存行为**中，选择**重定向 HTTP 到 HTTPS** 如果您已经设置了 SSL 证书。
4.  在**设置**中，选择一个自定义 SSL 证书(如果已创建)并将`index.html`输入到**默认根对象**中。
5.  点击**创建分销**

如果您在浏览器中转到分发域名，您应该能够看到您的网站(您可能需要等待几分钟)。

最后，如果您使用自定义域名，您应该创建一个指向分发域名的 CNAME 记录，这样您就可以通过您的自定义域名访问 CDN 上的网站。

## 结论

在本文中，我们能够创建一个自动化管道来将静态 Gatsby 网站部署到 S3。我们还能够添加一个自定义域，并在 CloudFront 发行版中托管我们的网站资产。一般来说，在 AWS 上托管任何静态网站都会遵循类似的基础设施模式。

将来，您可能希望在部署之前将测试阶段添加到您的管道中，进一步调整 CDN 等。希望这篇文章已经教了你足够的知识，让你开始在 AWS 上使用 CI/CD 和静态站点托管。