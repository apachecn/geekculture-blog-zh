# 设计革命性的网络应用👊第三部分

> 原文：<https://medium.com/geekculture/designing-a-revolutionary-web-app-part-iii-bb228d550897?source=collection_archive---------6----------------------->

![](img/cdaba3da5a8f550387a3e3dbbe6bca86.png)

Photo by [Conor Sexton](https://unsplash.com/@conorsexton?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/ocean?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 第三部分——devo PS/数字海洋

这是我的故事的第三部分，关于设计一个网络应用来支持苏丹人民抵抗政变。你可以在这里看到地点[。](https://sudan-art.com/)

你可以在这里看到第二部分。

首先，如果你想花 100 美元试用数字海洋 60 天，点击[这个](https://m.do.co/c/914e910aa17b)链接！

在本文的最后一部分，我将讨论将一整套 Django/React 应用程序与 docker compose 一起部署到数字海洋水滴的一些难点。

我认为很多观点可能与其他项目相关😅

以下是我将关注的内容:

1.  如何使用 docker compose 为 MVP 部署到数字海洋；
2.  如何在使用反向代理服务器的安全生产部署中服务 Django/React with NGINX；
3.  如何设置一个简单的 Github actions 工作流来自动化代码林挺、测试和部署的 CI/CD 管道。

## 如何使用 docker compose 为 MVP 部署到数字海洋

所以，这一部分并不复杂。我决定使用 docker compose，因为这个网站更多的是一个档案项目，我不希望有大量的流量。不需要 kubernetes😰

您只需在数字海洋水滴上运行 docker compose 实例，就万事大吉了——这是一个 linux 盒子！这也使它成为一种非常简单的部署方法。

这是我的生产 docker 合成文件的样子:

这就是这里发生的事情…我有三个服务:

*   Django:构建后端 Django 应用程序，并使用 gunicorn 在端口 8000 上提供服务。在数字海洋中，我不允许通过防火墙上的 8000 端口进行连接，所以你只能在 docker compose 网络内部访问这个端口；
*   NGINX:这个 Dockerfile 实际上是一个多阶段的构建，首先构建 react 应用程序，从中提供静态文件，然后将 api 请求路由到后端；
*   Certbot 这为我的站点更新了证书，所以它在 HTTPS 上工作。

我不打算转换 certbot 的部分，因为它可能值得自己的一块。但是你可以在这篇非常好的文章中读到更多关于如何做的内容。

以下是 Django 应用程序的 docker 文件:

它使用了一个像这样的小 shell 脚本:

如果我在开发东西，它让我运行 Django 开发服务器，但是如果它在生产中，确保我使用 gunicorn。

NGINX Dockerfile 文件有点复杂，如下所示:

这里的关键命令是第 15 行中的命令，您复制了构成第 9 行中的`npm run build`命令输出的所有捆绑文件，以便 NGINX 可以为它们服务。

现在开始真正棘手的部分💥

## 如何在使用反向代理服务器的安全生产部署中服务 Django/React 与 NGINX

下面是`nginx.conf`文件，它做了大量繁重的工作将所有这些联系在一起:

这里有相当多的事情要做，以确保网站是安全的，并尽可能多地压缩文件，所以不要担心乍一看感觉有点压倒性。

我认为理解这些文件的最简单的方法是弄清楚当用户的请求到达站点时会发生什么。如果他们访问 HTTP 上的端口 80，第 33 行会将他们重定向到相同的 url，但通过 HTTPS。然后这将它们带到第 54 行，在那里服务器在端口 443 上监听 HTTPS 请求，并将这些请求带到 React 项目，该项目实际上将`index.html`作为入口点。

第 57、51 和 65 行上的其他位置块都是我的 Django 应用程序中的路线。也就是说，只有当你能够访问 8000 端口时，你才能访问它们，而外部计算机则不能。这就是为什么他们都有这条线`try_files $uri @proxy_api;`，NGINX 的意思是“去看看定义为`@proxy_api`的相同端点”。第 58 行是唯一包含一些查询字符串的行，因为这是唯一需要查询字符串的 api 端点。

这将我们带到第 69 行，第 70 行中的`proxy_pass`块将到这个端点的请求路由到在端口 8000 上运行 Django 的 Docker 服务。NGINX *是*能够访问它，因为它是从同一个 docker compose 运行的，所以可以访问网络。

在我进入 devops 之前，有一些安全提示…

*   第 4 行很有帮助，因为它防止攻击者知道您使用的 NGINX 版本，而只是在 googling 上搜索该版本的漏洞。
*   第 47 行确保您使用更高版本的 TLS
*   第 48 行确保您使用 HSTS
*   第 49 行防止点击劫持攻击

最后但同样重要的是…

## 如何设置一个简单的 Github actions 工作流来自动化代码林挺、测试和部署的 CI/CD 管道

根据我的经验，Github actions 工作流的设置可能有点复杂，但一旦它们启动并运行起来，一切都会变得有趣得多。自动化代码林挺、测试、docker 构建和部署都意味着很难在生产中意外破坏你的应用(😱)并且您不必每次都手动进行部署。

这就是我当时的感觉。我想对主分支中的任何 pull 请求运行一系列集成测试，这是我将新代码添加到代码库中的唯一方法，如果我合并了 PR，那么就将新代码部署到我的 droplet 中。

因此，简单地说，第 2–6 行中的 on 块触发这个拉和推请求的工作流。但是第 68 行意味着部署作业只有在推送到 main 时才会运行😉第 67 行意味着它只有通过 lint/test/build 阶段才会运行，这将有助于防止 bug 进入生产环境。

第 12–23 行确保我的依赖项在每次运行时都被缓存，这真的加快了速度。它们也在第 37-50 行重复出现。然后，您可以运行适合您的林挺/测试需求的任何节点或 python 命令。有时这可能有点复杂——请看第 52 行，了解如何在 yaml 语法中拆分一个带有多个标志的长命令。如果你使用 pylint，获取所有的 python 文件可能有点麻烦，所以`git ls-files`是我的解决办法🙃

从第 71 行开始的部署部分有点复杂。你需要添加一些秘密到你的 github 库，这样你就可以 SSH 到 Digital Ocean，你还需要确保你的 droplet 启用了这些。这里的是一个关于如何做到这一点的很好的教程。

但是你走了！现在你只需要在你的 pull 请求上点击 squash 和 merge，新的代码就部署好了,(希望)不会破坏你的应用😍