# 停止支付您的 SSL 证书

> 原文：<https://medium.com/geekculture/stop-paying-for-your-ssl-certificates-55d6c5efebad?source=collection_archive---------5----------------------->

## 网络开发基础

## 了解如何停止为您支付 SSL 证书费用

![](img/775d1dd0db8b8da3cbf4a5a2069d1c1f.png)

Photo by [Markus Winkler](https://unsplash.com/@markuswinkler?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

您知道在您的网站上启用 HTTPS 无需支付 SSL 证书费用吗？与 Go-Daddy 和 BlueHost 的所有营销相反，SSL 证书不需要花费任何钱。网络托管公司向你收取任何费用的唯一原因是因为他们自己自动化了这个过程。但是，一旦您学会了如何安装 SSL 证书，您就可以自己自动化整个过程，从而为您节省时间和金钱。

**本指南假设您正在使用 Debian 和任何衍生操作系统以及最流行的操作系统 Apache web server 和 web server。到达“更多详细信息”部分后，您可以找到关于其他操作系统和网络服务器的更多信息。**

## 如何在单个域中启用 SSL？

您将需要对服务器的根访问权限。大多数受欢迎的主机提供商不会为网站提供根访问，尤其是不便宜的。以下是一些主机提供商，他们确实以低成本提供虚拟专用服务器。

[](https://digitalocean.com) [## 数字海洋——开发者云

### 当开发人员能够在他们喜爱的简单、平价的云基础上构建时，业务增长会更快。数字海洋有云……

digitalocean.com](https://digitalocean.com) [](https://vultr.com) [## Vultr 的 SSD VPS 服务器、云服务器和云托管

### Vultr 控制面板使服务器管理简单直观。常见任务，如订购服务器、管理…

vultr.com](https://vultr.com)  [## VPS &共享网络托管服务包- 1984 年网络托管

### 1984 年的虚拟专用服务器非常可靠，非常稳定。我们的虚拟专用网络运行在企业级硬件上，位于…

1984hosting.com](https://1984hosting.com) 

一旦您拥有对服务器的根访问权限，就需要安装快照。是的，您需要在已经有包管理器的操作系统上安装另一个包管理器。[你可以读到我对通用包装经理的不满。](/geekculture/why-universal-package-managers-are-stupid-521f6ceb2907)

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install snapd
sudo snap install core
sudo snap refresh core
```

使用从您的操作系统中删除 certbot 的任何其他副本

```
sudo apt-get remove certbot
```

安装 certbot 的新快照版本

```
sudo snap install --classic certbot
```

链接你的命令，这样他们就可以访问 certbot 系统

```
sudo ln -s /snap/bin/certbot /usr/bin/certbot
```

您可以运行以下命令将 certbot 证书自动安装到 web 服务器中。

```
sudo certbot --apache
```

或者您可以获取生成证书并使用

```
sudo certbot certonly --apache
```

虽然大多数付费 SSL 证书都允许您将证书保存一至两年。这仍然意味着您必须每年购买证书并重新安装。有时，您可能会遇到软件不能自动更新应用程序的问题。Certbot 已经通过内置的自动更新功能解决了这个问题。使用以下命令测试自动续保。

```
sudo certbot renew --dry-run
```

如果 certbot 自动续保失败，请继续阅读，您将了解如何解决此问题。

[在这里注册我的电子邮件列表。](/subscribe/@drechang)

## 如何启用 SSL 域通配符？

通配符基本上是您原始域的一个子域。

```
*.example.tld
```

也许你正在你的域上运行多种服务，比如电子邮件服务器、分析服务器、聊天服务器等。因此，你需要在你购买的同一个域名上有子域名，以便于组织你网站的别名。您应该将您的服务器设置为位于它后面的其他服务的反向代理。

唯一的区别是，在链接命令之后，您需要执行这些额外的步骤。

您需要使用以下命令来启用插件支持

```
sudo snap set certbot trust-plugin-with-root=ok
```

然后你需要安装 DNS 插件

```
sudo snap install certbot-dns-[plugin]
```

您还需要在您的服务器上设置凭证，以便 certbot 服务器可以通过您的服务器进行身份验证。以下是支持的 DNS 列表以及如何设置凭据。

 [## 用户指南- Certbot 1.23.0 文档

### Certbot 使用许多不同的命令(也称为“子命令”)来请求特定的操作，例如…

eff-certbot.readthedocs.io](https://eff-certbot.readthedocs.io/en/stable/using.html#dns-plugins) 

在这里注册我的电子邮件列表。

## 更多细节

如果您正在使用不同的操作系统或 web 服务器，请查看以下指南，它将允许您

[](https://certbot.eff.org/instructions) [## 证书机器人说明

### 品牌理念;标签行

certbot.eff.org](https://certbot.eff.org/instructions) 

[在这里注册我的电子邮件列表。](/subscribe/@drechang)

## 如何修复自动更新问题？

解决自动更新失败的最佳方法是阅读问题并在互联网上查找以解决问题。但是，如果您仍然不能解决这些问题，那么您必须通过以下步骤手动自动续订。

创建将停止服务并续订证书的脚本。

```
#!/bin/bash
systemctl stop [web_service]
sudo certbot renew
systemctl start [web_service]
```

然后，您可以设置一个 crontab 作业，例如

```
0 3 * * * [path_to_script] # Automates SSL updates at 3 AM UTC everyday
```

不要担心每天不断改变你的 SSL 证书，因为 certbot 有一个内置的功能，可以防止你经常更新。如果你想了解 crontab，你可以在这里阅读关于它的[。您还可以使用这个免费的 web 服务来查看您的 crontab 输入在简单英语中是什么意思。](/geekculture/crontab-automate-server-processes-b246f47cf709)

 [## crontab . guru——cron 调度表达式编辑器

### cron 调度表达式的快速简单的编辑器我们创建了 Cronitor，因为 cron 本身不能…

crontab.guru](https://crontab.guru/) 

在这里注册我的电子邮件列表。

**相关内容:**

*   [如何建立一个基于内容的分发网络？](/@drechang/how-to-build-a-based-content-delivery-network-e1aa8bb237b3)
*   [如何备份您的媒体状态？](/@drechang/how-to-build-a-based-content-delivery-network-e1aa8bb237b3)
*   [去谷歌化的完整指南](/@drechang/how-to-dismantle-the-google-empire-e652bff6d2)