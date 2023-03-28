# 在 Circle CI 中配置 AWS 身份证明

> 原文：<https://medium.com/geekculture/configure-aws-credentials-in-circle-ci-8353d765aa15?source=collection_archive---------2----------------------->

![](img/595428c094fcbb8cee59c09aa724a823.png)

Source([link](https://circleci.com/))

在这篇简短的帖子中，我将带您浏览在 [Circle CI](https://circleci.com/) 中配置 AWS 凭证的步骤。

# 先决条件

您首先需要使用您的 AWS 帐户生成访问密钥和密码，如果您还没有这样做的话。为此，请访问以下链接:

*   [在你的 AWS 账户中创建一个 IAM 用户](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_users_create.html)
*   [定义用户权限](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html#grant-least-privilege) …