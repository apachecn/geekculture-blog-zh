# 用 NGINX 设置私有 PyPI 服务器

> 原文：<https://medium.com/geekculture/setting-a-private-pypi-server-with-nginx-acbb73c8516d?source=collection_archive---------1----------------------->

## 在本文中，我们将使用 [pypiserver Docker 映像](https://hub.docker.com/r/pypiserver/pypiserver)建立一个私有的 PyPI 服务器，该映像将由 [NGINX](https://www.nginx.com/) 包装，用于缓存和性能。

![](img/421d7cba2af469c93337c9ed4d72bd1a.png)

Photo by [Spencer Judd](https://unsplash.com/@stjudd109) on [Unsplash](https://unsplash.com/)

## **PyPI 是什么？**

[正式文件](https://pypi.org/):

> Python 包索引(PyPI)是 Python 编程语言的软件仓库。

PyPI 是 Python 语言的官方第三方软件库。

要从 PyPI 存储库中安装软件包，您需要一个软件包安装程序。最常见最推荐的是 [pip](https://pypi.org/project/pip/) 。

PyPI 是公开的，任何人都可以访问它并下载上面列出的任何包。但是，有时您希望创建自己的私有存储库，这主要是出于安全原因。

## Docker 设置

要建立一个 PyPI 服务器，你需要一个服务器——它可以是你自己的机器，你朋友的，或者云中的机器(AWS，GCP 等等)。在本教程中，我假设您已经有了一台服务器来托管您的私有存储库。

因为我们将使用 Docker 来运行服务器，所以我们需要安装 Docker:

Docker installation

## PyPI 服务器安装

让我们为我们的 PyPI 配置创建一个新目录。

```
# replace with your name
mkdir /home/maroun/pypi-server
```

创建`docker-compose.yml`:

请注意，我们目前通过指定`-P . -a .`命令来允许未经授权的访问。

现在，我们应该配置我们的 NGINX 服务器。在`pypi-server`中，创建以下目录:

```
mkdir nginx nginx_cache
mkdir nginx/conf.d
```

现在，添加`pypi.conf`的`conf.d`，内容如下:l

我们监听端口 80(默认端口)，并将所有请求传递给“pypi-server:8080”，我们的 pypi 服务器在这里运行。

默认情况下，Compose 为我们的应用程序建立了一个单独的[网络](https://docs.docker.com/engine/reference/commandline/network_create/)。服务的每个容器都加入默认网络，并且对于该网络上的其他容器都是*可到达的*，并且在与容器名称相同的主机名处被它们*发现*。这就是为什么我们可以在上面的配置中使用`pypi-server`主机。

最后，让我们在`nginx`目录中添加(默认)`nginx.conf`，它包括我们上面的自定义配置(第 27 行):

nginx.conf

## 启动服务器

现在，我们应该能够从`pypi-server`目录运行以下内容:

```
docker-compose up -d
```

从服务器访问本地路由:

```
$ curl localhost:8080
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Welcome to pypiserver!</title>
  </head>
  <body>
    <h1>
      Welcome to pypiserver!
    </h1>
    <p>
      This is a PyPI compatible package index serving 11 packages.
    </p>
    <p>
      To use this server with <code>pip</code>, run the following command:
      <pre>
        <code>pip install --index-url [http://pypi-server:8080/simple/](http://pypi-server:8080/simple/) PACKAGE [PACKAGE2...]</code>
      </pre>
    </p>
    <p>
      To use this server with <code>easy_install</code>, run the following command:
      <pre>
        <code>easy_install --index-url [http://pypi-server:8080/simple/](http://pypi-server:8080/simple/) PACKAGE [PACKAGE2...]</code>
      </pre>
    </p>
    <p>
      The complete list of all packages can be found <a href="/packages/">here</a> or via the <a href="/simple/">simple</a> index.
    </p>
    <p>
      This instance is running version 1.4.2 of the <a href="[https://pypi.org/project/pypiserver/](https://pypi.org/project/pypiserver/)">pypiserver</a> software.
    </p>
  </body>
</html>
```

## 下载包

使用`pip`，你现在可以从你自己的私有库下载包了！

```
pip install --index-url http://<IP>:8080 my_package --trusted-host <IP>
```

可以使用 [Twine](https://twine.readthedocs.io/en/latest/) 将包发布到我们新创建的服务器上。请继续查看它的文档以获得更多信息。

## 认证请求

当我们现在使用我们的私有存储库时，任何可以访问我们服务器的人都可以与存储库进行交互。为了保护它，我们可以使用`[htpasswd](https://github.com/pypiserver/pypiserver#apache-like-authentication-htpasswd)`。安装`httpd-tools`:

```
sudo yum install -y httpd-tools
```

现在，创建一个用于身份验证的文件夹:

```
mkdir authentication
```

并创建一个用户名:

```
# create .htpasswd file and force SHA encryption
htpasswd -sc .htpasswd pypi-user
```

系统将提示您插入密码。完成后，`.htpasswd`将为新用户创建一个条目，使用哈希密码:

```
pypi-user:{SHA}s91+2DUySxI8K+YpcFDJPbI596c=
```

接下来，我们应该更新 Docker 编写文件:

Docker compose YML

注意，对于更新、下载和列表命令，我们将命令更改为 [require authentication](https://github.com/pypiserver/pypiserver#table-of-contents) 。您可以查看`pypi-server`命令的手册:

```
pypi-server understands the following options:

  -p, --port PORT
    Listen on port PORT (default: 8080).

  -i, --interface INTERFACE
    Listen on interface INTERFACE (default: 0.0.0.0, any interface).

  -a, --authenticate (update|download|list), ...
    Comma-separated list of (case-insensitive) actions to authenticate.
    Requires to have set the password (-P option).
    To password-protect package downloads (in addition to uploads) while
    leaving listings public, use:
      -P foo/htpasswd.txt -a update,download
    To allow unauthorized access, use:
      -P . -a .
    Note that when uploads are not protected, the `register` command
    is not necessary, but `~/.pypirc` still need username and password fields,
    even if bogus.
    By default, only 'update' is password-protected.
```

访问 PyPI 服务器现在需要认证。

## 摘要

有时你不想把你的包放在一个公共存储库中。创建一个定制的 PyPI 服务器是一个很好的主意，可以提高安全性，并且不会将您的包暴露给任何人。

使用 NGINX，我们引入了一种缓存机制，并且我们可以在需要时轻松地配置负载平衡器。