# 如何使用 AWS Route 53、S3 和 CloudFront 托管静态网站

> 原文：<https://medium.com/geekculture/how-to-host-a-static-website-using-aws-route-53-s3-and-cloudfront-e425fa5de349?source=collection_archive---------3----------------------->

## 虚拟主机教程

## 这是一个循序渐进的教程，讲述了如何在 Route 53 上注册域名，在 S3 上托管网站，并使用 CloudFront 保护它们

![](img/c001a38383855fcaf90bea8a77f56ac1.png)

Photo by [Austin Distel](https://unsplash.com/@austindistel?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 介绍

在我之前的 Medium 文章中，我向用户展示了如何创建静态网站。

当网站开发工作完成后，网站需要在线托管，以便可以与世界共享，尽管有许多平台可以托管静态网站，但亚马逊网络服务(AWS)提供了最实惠和可扩展的选择之一[1]！

根据 Bluestack 的说法，以下是在 AWS 上托管静态网站是一个好主意的几个原因[2]:

1.  **自动扩展**—AWS S3 的一个特点是它可以自动扩展以处理大量请求，因此用户不必担心高流量。
2.  **高可用性** —由于亚马逊保证 AWS S3 99.99%的可用性，用户的网站将默认 99.99%的可用性。
3.  **低成本** —虽然用户可以通过使用 AWS 月度计算器获得准确的成本估计，但平均每月 30，000 次请求将使用户每月花费约 1 美元的 AWS 成本，并且比其他托管选项便宜得多。

在本教程中，我将向用户展示如何利用 AWS 服务，以便他们可以使用 Route 53 注册域名，在 S3 上托管网站，并使用 CloudFront 保护它们！

# AWS 服务

![](img/7c09de64cf40f3aa2dfb9bc831c23a51.png)

Photo by [Christian Wiediger](https://unsplash.com/@christianw?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

一般来说，托管网站有三个组成部分:

1.  注册自定义域
2.  托管网站的源代码
3.  保护网站的访问

幸运的是，AWS 有几个不同的服务来帮助这些组件！

## 向 AWS 路由 53 注册域

公开托管网站的第一步是注册自定义域名，这样用户就不会被迫使用默认的 AWS S3 域名作为他们网站的 URL。

幸运的是，AWS 为用户提供了通过 Route 53 服务注册域名的能力[3]。

以下是用户配置 Route 53[4]时可以遵循的步骤:

1.  转到 AWS 控制台
2.  导航到 AWS Route 53 服务
3.  决定自定义域名
4.  选择他们的网站需要哪些子域
5.  为每个子域创建 Route 53 记录，这些记录将在最后一节中用于指向 CloudFront 发行版

## 在 S3 AWS 上托管网站

既然域名已经注册，用户现在可以开始正确设置 AWS S3 桶来托管他们的网站[5]。

用户需要按照以下步骤正确设置他们的 S3 桶[6]:

1.  转到 AWS 控制台
2.  导航到 AWS S3 服务
3.  创建公共 S3 存储桶
4.  为托管网站设置 S3 存储桶
5.  创建一个样本 HTML 文件来测试 S3 桶是否设置正确[7]
6.  访问公共 S3 URL，并验证示例 HTML 文件是否正确显示
7.  上传实际的网站源代码，但请记住，S3 桶在这一点上是公开的，所以桶中的一切都可以被任何人看到！

## 使用 AWS CloudFront 保护网站访问

由于该网站现在可以公开访问，用户需要使用 AWS CloudFront 来保护它，并使用世界各地边缘位置的缓存来实现快速访问[8]。

为了正确设置 CloudFront 发行版，用户应该遵循以下步骤[9]:

1.  转到 AWS 控制台
2.  导航到 AWS CloudFront 服务并启动 CloudFront 发行版创建过程
3.  为您的分配指定必要的参数，并使用上一节中的 S3 存储桶信息:
4.  将 S3 bucket 设为私有，只允许从 CloudFront 访问这个 bucket(最好启用这个安全特性，并禁用对 S3 bucket 的公共访问)
5.  更新设置以要求查看器和 CloudFront 之间的 HTTPS
6.  创建一个可由 CloudFront 使用的 SSL 证书
7.  更新 CloudFront 分发设置，以便从第一部分中创建的 Route 53 记录中访问
8.  通过自定义域名测试到网站的连接，并确认对 S3 存储桶的公共访问已禁用。

# 结论

![](img/7d7b37a8fe01bb80c69ad6d189b4b3d3.png)

Photo by [bruce mars](https://unsplash.com/@brucemars?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

恭喜你！！！该网站现已成功托管在 AWS 上，任何人都可以在其自定义域上访问它！

总的来说，有很多方法可以托管静态网站，但是如果用户想使用 AWS 托管他们的网站，这里有三种服务:

1.  **Route 53** —用户可以为自己的网站注册自定义域名。
2.  **S3**——用户可以利用 AWS 的原生 S3 特性来处理他们网站的性能和可扩展性。
3.  **CloudFront** —用户可以安全访问他们的网站，并在全球范围内实现快速访问。

下面是一个使用 53 号公路、S3 和 CloudFront 托管在 AWS 上的网站示例:

[](https://www.deeptaanshu.com/) [## 迪普塔安舒·库马尔

### 欢迎来到我的网站！请看看我的工作，并随时与我联系！

www.deeptaanshu.com](https://www.deeptaanshu.com/) 

希望这篇教程对你有所帮助，也能启发其他人创建/托管他们自己的网站！！！

# 参考

[1]什么是 AWS:[https://aws.amazon.com/https://aws.amazon.com/](https://aws.amazon.com/)

[2]为什么应该使用 AWS S3 托管网站:[https://www . blue stack cloud . com/insights/2018/5/7/advantages-of-hosting-your-static-website-on-Amazon-S3](https://www.bluestackcloud.com/insights/2018/5/7/advantages-of-hosting-your-static-website-on-amazon-s3)

[3] AWS 路线 53:[https://aws.amazon.com/route53/](https://aws.amazon.com/route53/)

[4]设置自定义域:[https://docs . AWS . Amazon . com/Amazon S3/latest/dev/website-hosting-custom-domain-walk through . html](https://docs.aws.amazon.com/AmazonS3/latest/dev/website-hosting-custom-domain-walkthrough.html)

[5]AWS https://aws.amazon.com/s3/ S3:

[6]设置 S3 桶:[https://docs . AWS . Amazon . com/Amazon S3/latest/user-guide/static-website-hosting . html](https://docs.aws.amazon.com/AmazonS3/latest/user-guide/static-website-hosting.html)

[7]样本 HTML 文件:[https://www.w3schools.com/html/html_basic.asp](https://www.w3schools.com/html/html_basic.asp)

[8]AWS CloudFront:[https://aws.amazon.com/cloudfront/](https://aws.amazon.com/cloudfront/)

[9]设置 CloudFront 发行版:[https://docs . AWS . Amazon . com/Amazon CloudFront/latest/developer guide/distribution-we b-values-specify . html](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/distribution-web-values-specify.html)