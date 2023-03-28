# Tyk API 网关入门

> 原文：<https://medium.com/geekculture/get-started-with-tyk-api-gateway-aae127186e1b?source=collection_archive---------1----------------------->

![](img/a086e5802aeeca65e1203271fb0c1386.png)

Photo by [winui](https://www.shutterstock.com/g/winui) on [shutterstock.com](https://www.shutterstock.com/)

[Tyk](https://tyk.io/) 提供各种产品来管理 API 和服务。其产品面板的核心是其开源的 API 网关。它完全是在 Go 中开发的，可以作为 REST、GraphQL 和其他服务的代理。本指南描述了如何使用 Docker 运行 API 网关。

# 你需要什么

*   码头工人
*   REST 客户端——在我的例子中我将使用 [HTTPie](https://httpie.io/) ,但是任何客户端都可以工作

# 用 Docker 运行 Tyk 网关

要启动并运行 Tyk Gateway，您需要运行 [Redis](https://hub.docker.com/_/redis) ，当然还有 [Tyk Gateway](https://hub.docker.com/r/tykio/tyk-gateway/) 本身。按照下面的说明，让一切工作。

**1。配置网络**

```
$ docker network create tyk
```

**2。拉出并运行 Redis** 我使用的是 Redis 5.0.10 而不是最新版本，因为根据 [Tyk 文档](https://tyk.io/docs/planning-for-production/redis-mongodb/)这是最新支持的版本。请注意，像这样运行 Redis 并不是生产环境的首选方式。Redis 应该高度可用，并作为集群部署。

```
$ docker pull redis:5.0.10
$ docker run -itd --rm --name redis --network tyk -p 6379:6379 redis:5.0.10
```

**3。创建 Tyk 配置**

创建一个新目录，它将保存您的配置和 API 定义。此外，创建一个包含以下 JSON 的新文件。我建议用`tyk.conf`作为这个文件的名字。由于我们使用的是 Tyk API Gateway，没有任何其他(企业)产品，因此该配置基于[这个存储库](https://github.com/TykTechnologies/tyk-gateway-docker)中的`tyk.standalone.conf`。我删除了一些属性，这些属性现在并不重要。我做的最重要的改变是移除了`secret`，因为我们将通过一个环境变量来设置它，以避免在配置中暴露它。

大多数值都是不言自明的。阅读 Tyk 的文档以了解更多信息。

**4。拉动并运行 Tyk 网关**

现在是运行 API 网关的时候了。执行下面打印的命令，为 API 网关提取 Docker 映像并运行它。

```
$ docker pull tykio/tyk-gateway:latest
$ docker run -d \
  --name tyk_gateway \
  --network tyk \
  -p 8080:8080 \
  -e TYK_GW_SECRET=[YOUR-SECRET] \
  -v $(pwd)/tyk.conf:/opt/tyk-gateway/tyk.conf \
  -v $(pwd)/apps:/opt/tyk-gateway/apps \
  tykio/tyk-gateway:latest
```

下面对`run`命令的参数进行解释:

*   `--name tyk_gateway`:设置容器的名称。
*   `--network tyk`:将容器连接到网络`tyk`。
*   `-p 8080:8080`:将容器的端口 8080 暴露给系统的端口 8080。
*   `-e TYK_GW_SECRET=[YOUR-SECRET]`:设置保存你的秘密的环境变量。
*   `-v $(pwd)/tyk.conf:/opt/tyk-gateway/tyk.conf` : Bind 将我们在上一步中创建的配置挂载到容器内的配置文件中。
*   `-v $(pwd)/apps:/opt/tyk-gateway/apps` : Bind 将我们当前文件夹中的 app 目录挂载到容器内的目录`opt/tyk-gateway/apps`。

**5。验证所有设置是否正确**

要验证 API 网关已经启动并运行，您可以简单地向`/hello`发送一个`GET`请求。答案应该类似于下面打印的 JSON。

# 编辑 Tyk 的配置

编辑您的配置非常容易。因为我们为配置创建了一个绑定挂载，所以您可以简单地编辑我们之前创建的配置文件来进行配置更改。

> 注意: [Vim](https://www.vim.org/) 并不直接保存一个文件，而是创建一个新的文件并复制到适当的位置。因此，文件的信息节点正在更改，保存文件会中断绑定装载。为了避免这种情况，您需要在 vim 中设置`set backupcopy=yes`。如果您想使该设置永久化，您需要将其添加到文件`~/.vimrc`中。如果这个文件不存在，您可以简单地创建它，vim 会注意到它。

现在剩下要做的就是重新加载 API 网关。这可以通过`GET`调用`/tyk/reload`来实现。

# 添加 API

现在，API 网关已经启动并运行，是时候向它添加 API 了。您既可以通过向 API 网关的 REST API 发出请求来添加 API，也可以通过将 API 定义添加到文件夹`/opt/tyk-gateway/apps`来添加 API，我们之前已经将该文件夹挂载到了`./apps`目录中。在本指南中，我将使用第二个选项，因为我更喜欢可配置的文件。如果您选择第一种可能性，您可以简单地向`/tyk/apis`发送一个`POST`请求，其中包含 API 定义。请不要忘记设置该请求中的`x-tyk-authorization`标题。

我们将添加一个在`/httpbin`可用的 API，并将请求代理到`[https://httpbin.org](https://httpbin.org.)`。这个 API 的最小配置可以在下面的要点中找到。同样，所有属性的描述可以在 [Tyk 的文档](https://tyk.io/docs/tyk-gateway-api/api-definition-objects/)中找到。

导航到为您创建的`./apps`目录 Docker，并创建一个包含上述 JSON 的新文件`httpbin.json`。

添加新端点的最后一步是用下面的请求重新加载 API 网关。

```
$ http :8080/tyk/reload 'x-tyk-authorization:[YOUR-SECRET]'
```

就是这样。正如你在下面的要点中看到的，对`localhost:8080/httpbin`的请求现在被代理到`httpbin.org`。

# 使用 Tyk 进行认证

有几种方法可以实现 API 的身份验证。Tyk 支持以下认证方法。从 Tyk 2.3 版开始，还可以链接认证中间件。

*   基本认证
*   不记名令牌
*   HMAC 请求签署
*   JSON Web 令牌
*   OpenID 连接

Tyk 还提供对 OAuth 2.0 的支持，甚至可以作为授权和访问令牌的提供者。

由于本指南旨在帮助您开始使用 Tyk，因此本节没有涵盖所有的认证和授权方法。为了对身份验证的工作原理有一个大致的了解，在接下来的小节中将解释不记名令牌和基本身份验证。

## 不记名令牌

不记名令牌认证是用 Tyk 保护 API 的默认方式。在我们之前添加的 API 的定义中，我们将`use_keyless`属性设置为`true`来公开 API 而不进行身份验证。下面你可以看到一个 API 定义，带有不记名令牌认证。它非常类似于我们上面使用的定义。`name`、`listen_path`和`id`的值已更改，并且`use_keyless`已被删除。

如果重新加载 API 网关，对`/httpbin`的请求仍然像以前一样工作，因为它们属于先前创建的 API。如果你向`/httpbin/bearer`发出请求，API 网关将返回`401 Unauthorized`，并显示错误`Authorization field missing`。在将`Authorization`头设置为任意值后(我们将马上发布一个令牌)，网关将返回`403 Forbidden` — `Access to this API has been disallowed`。

API 现在已经准备好了，因此我们需要发布一个令牌来访问它。为此，我们需要定义一个会话对象。下面给出了一个最小的定义。它授予对组织 1 中 ID 为 2 的 API(我们刚刚创建的 API)的访问权。使用如下所示的会话对象，您可以配置很多东西。查看官方文件以了解更多。

现在向`/tyk/keys`发送带有 POST 请求的会话对象，以发布认证令牌。

您的令牌包含在响应的`key`字段中。它需要在对`/httpbin/bearer`的所有请求的`Authorization`报头中发送。

## 基本认证

为了配置基本身份验证，我们将创建一个新的 API。定义看起来和你已经知道的相似，但是`name`、`id`和`listen_path`改变了。最重要的变化是添加了字段`use_basic_auth`并设置为`true`。这告诉 Tyk 使用基本 Auth 而不是承载令牌来保护这个端点。您不需要配置其他任何东西，因为 Tyk 知道如何处理基本的 Auth 标准。

重新加载 API 网关后，如果没有传递用户或传递了无效用户，对`httpbin/basic`的请求将会失败。要创建新用户，我们需要创建一个会话令牌。您可以在下面的代码片段中看到这个令牌的配置。同样，这个定义非常类似于我们用来发布不记名令牌的定义。当然，API 的`name`和`id`发生了变化，因为我们现在想要为 API 3 创建一个用户。此外，在`basic_auth_data`中添加了用户的密码。

现在一切都准备好了，可以通过对`/tyk/keys/[USERNAME]`的 POST 请求将用户发布到 API 网关。下面的代码片段展示了如何创建一个名为`a-user`的新用户。

现在，您可以使用新创建的凭证发出请求。

# 结论

在这篇简介中，我们介绍了如何使用 Docker 启动和运行 Tyk API 网关，以及如何定义基本的 API 和简单的认证机制。

就是这样。非常感谢你阅读这篇文章！希望对你进入 Tyk API 网关有所帮助。

# 资源

[Tyk API 网关](https://github.com/TykTechnologies/tyk)

[HTTPie](https://httpie.io/)

[Redis](https://hub.docker.com/_/redis) 和 [Tyk 网关](https://hub.docker.com/r/tykio/tyk-gateway/)的 Docker 图像

[Tyk 文档](https://tyk.io/docs/)。