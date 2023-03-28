# 使用 Sourcetree 无密码连接 Github

> 原文：<https://medium.com/geekculture/using-personal-access-token-in-sourcetree-to-connect-to-github-3702a29554d3?source=collection_archive---------1----------------------->

## 诀窍是这样的

## 如何在 Sourcetree 中使用个人访问令牌

![](img/3d8f792dbc9d3e6558afb1cec141ce6a.png)

在本文中，我将展示在 GitHub 不再支持用户名和密码认证后，使用 Sourcetree 中的 PAT(个人访问令牌)登录 GitHub 的两种方法。

## **背景**

![](img/3be5e6c82da036e1fe4cc3042c0e12d9.png)

Error Message when using Sourcetree to push to GitHub

如果你在克隆/推送过程中遇到了如下认证问题，请继续阅读，我会告诉你如何解决。错误消息称密码验证已于 2021 年 8 月 13 日移除，它还提供了一个包含更多信息的[链接](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/)。它包含一个标题为“Git 操作的令牌认证要求”的公告。如果你通读，你会发现“Sourcetree”受到影响，因为它是在受影响的工作流之一:“任何应用程序/服务访问 GitHub.com 上的 Git 库直接使用您的密码”。

## **先决条件:生成 PAT 令牌:**

现在，我将首先展示如何在 GitHub 中生成个人访问令牌:

1.  在 GitHub.com 页面，进入设置->开发者设置->个人访问令牌，点击“生成新令牌”

![](img/65da4f0d9383cf56fc32b61133e8e5f5.png)

Click ‘Generate new token’

2.在范围中，选中“repo”以获得对私有存储库的完全控制，并为令牌命名，例如 sourcetree access。单击页面底部的“生成令牌”按钮。

![](img/8ebe908536208abc1058dcf7dde558d2.png)

check ‘repo’ scope and click ‘Generate token’

3.令牌已创建，令牌值在此页面中仅显示一次，请将其复制到安全的地方以备后用。

![](img/3b54438fd9efb6379c1df4409f8b2020.png)

the token value will be shown only once, copy to secure place for use later

## **方法 1:在 git 克隆 URL 中使用 PAT 令牌:**

基于每个存储库(URL)进行身份验证的一种方法是将其放在存储库设置中的 git 克隆 URL 中。在 Sourcetree 中，转到“存储库”->“存储库设置”，转到“远程”选项卡，单击您要连接的远程的 URL(例如“源”)，使用以下模式修改 URL:

[https://GHP _ ryxuwm 79 xkatg 51 gy 7402 T3 jj mug 13 qdj 4n @ github . com/<user-id>/<repository-name>。git](https://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@github.com/thomas-wu2/coding-problem.git)

(在我的例子中，我的 PAT 是[GHP _ ryxuwm 79 xkatg 51 gy 7402 T3 ijjmug 13 qdj 4n](https://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@github.com/thomas-wu2/coding-problem.git))

![](img/7a27656f731d52fa511639e487bed1b1.png)

in Repo Setting, Adding PAT with @ followed by repo URL

再次尝试推送回购，这次成功完成。

![](img/c3ce7d442b007279062b0c0a7dcc5549.png)

## **方法 2:在 Sourcetree 账户中设置，对所有仓库有效**

在方法 1 中，我们需要为每个存储库设置 repo URL。这一次，我想展示一种更方便的方法:设置一次身份验证，并为 sourcetree 中的所有存储库工作。

方法 2 的想法是基于在命令行上使用令牌的特殊用法

首先，我们撤销方法 1 中的回购设置。然后，我通过在提交之后不推送来确保它不工作。

![](img/3be5e6c82da036e1fe4cc3042c0e12d9.png)

Error Message when using Sourcetree to push to GitHub

W 由于身份验证不起作用，现在我们可以进行设置了:转到 Sourcetree - >首选项- >帐户，点击“添加”。然后，在主机下拉列表中选择‘GitHub’；在 AuthType 下拉菜单中选择‘基本’(OAuth 不起作用)；在用户名中，输入您的 github 帐户名称。在**密码**中，粘贴个人访问令牌的值(我的例子是 GHP _ ryxuwm 79 xkatg 51 gy 7402 T3 ijjmug 13 qdj 4n)；在协议下拉列表中，选择 HTTPS，点击“保存”。

![](img/b17ed281ae53a33dc7521d02c2bb44f4.png)

账户创建如下:

![](img/519626989e74fbd5b3888d667b64c938.png)

an account is created

万岁！我们成功推动了变革！

![](img/89e550aecaea2b758dd36a63ce9f6986.png)

## **总结**

在本文中，我展示了如何在 Sourcetree 中使用 PAT 连接 GitHub。

希望你喜欢它，我会有其他的分享。

## **参考:**

1.  Git 操作的令牌认证要求:[https://github . blog/2020-12-15-token-authentic ation-Requirements-for-Git-Operations/](https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/)

2.在命令行上使用令牌:[https://docs . github . com/en/authentic ation/keeping-your-account-and-data-secure/creating-a-personal-access-token #在命令行上使用令牌](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/creating-a-personal-access-token#using-a-token-on-the-command-line)