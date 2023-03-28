# Docker 中的 Webapp + Nginx 和 SSL 构成

> 原文：<https://medium.com/geekculture/webapp-nginx-and-ssl-in-docker-compose-6d02bdbe8fa0?source=collection_archive---------0----------------------->

Python+Flask 中的一个简单例子，可以很容易地适应不同的上下文。

![](img/f8469ba5d0bc07e412c52372cbf8d16a.png)

Photo by [Christina @ wocintechchat.com](https://unsplash.com/@wocintechchat?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/server?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

本教程将解释如何创建一个简单的 web 应用程序，用 Nginx 创建一个代理，并用 Docker Compose 将它们连接在一起。web 应用程序将在 Python + Flask 中，但很容易用另一种技术替换它。

## 网络应用

web 应用程序只不过是一个简单的例子，它将`/`链接到`hello()`以打印“hello world！”当用户调用它时。

## 码头工人

Docker 是一种虚拟化技术，它将应用程序及其依赖项打包在一个独立于平台的容器中。我建议安装包含所有必要组件的 Docker 桌面。

一个`Dockerfile`是 Docker 用来构建一个`image`的一组指令，一个映像是 Docker 用来创建一个容器的只读包。

这是本教程的 docker 文件:

简而言之，所有 Dockerfiles 都从一个基本映像(本例中是 python 3.9)开始，复制一些文件，并运行一个或多个命令来设置和启动主进程。`setup.py`包含 python 应用程序的依赖关系，而`pip3`执行设置。最后一行以 Flask 开头。

## Nginx

Nginx 是一个 web 服务器，通常用作负载平衡器或代理。在这种情况下，启用 HTTPS 的代理将对与客户端的通信进行加密。

该配置启用 SSL 并配置`/`到`[http://app:5000](http://app:5000)`的转发。证书和 url `[http://app](http://appi)`将在后面定义。

## Docker 撰写

Docker Compose 是一个定义多容器应用程序的工具。在这种情况下，它将在一个虚拟网络中连接 Nginx 和 Flask，只暴露代理的端口。

这个配置创建了两个容器，`app`和`nginx`。`app`是`flask`微服务的映像，`nginx`是 Nginx 的一个实例，有三个卷来映射上面提到的配置，以及用于加密的证书/密钥。请注意，以这种方式定义的`app`将可以在虚拟网络内通过`[http://app](http://app)`到达。

## Run.sh

唯一缺少的部分是 TLS 证书和 Docker 映像的构建。这可以通过启动应用程序的脚本自动完成。

第一行生成一个自签名证书。这可以由静态耦合证书/密钥来代替。第二行构建了`Dockerfile`中描述的 docker 映像，最后一行启动了`docker-compose.yml`中描述的 Docker Compose。

![](img/a2b1df05263aa78ec40ee2e4032f427b.png)

The complete system

上图描述了该系统。用户在到达 Nginx 的`[https://localhost](https://localhost)`上连接，Nginx 卸载加密并将未加密的调用转发给 Flask 微服务。

# 试验

打开 [https://localhost](https://localhost) 可能会在大多数浏览器上出错，因为证书是自签名的。

在替代方案中`curl --insecure https://localhost`将简单地返回“hello world！”。详细输出显示了加密的用法。

```
$ curl --insecure https://localhost -v
*   Trying ::1...
* TCP_NODELAY set
* Connected to localhost (::1) port 443 (#0)
* ALPN, offering h2
* ALPN, offering http/1.1
* successfully set certificate verify locations:
*   CAfile: /etc/ssl/cert.pem
  CApath: none
* TLSv1.2 (OUT), TLS handshake, Client hello (1):
* TLSv1.2 (IN), TLS handshake, Server hello (2):
* TLSv1.2 (IN), TLS handshake, Certificate (11):
* TLSv1.2 (IN), TLS handshake, Server key exchange (12):
* TLSv1.2 (IN), TLS handshake, Server finished (14):
* TLSv1.2 (OUT), TLS handshake, Client key exchange (16):
* TLSv1.2 (OUT), TLS change cipher, Change cipher spec (1):
* TLSv1.2 (OUT), TLS handshake, Finished (20):
* TLSv1.2 (IN), TLS change cipher, Change cipher spec (1):
* TLSv1.2 (IN), TLS handshake, Finished (20):
* SSL connection using TLSv1.2 / ECDHE-RSA-AES256-GCM-SHA384
* ALPN, server accepted to use http/1.1
* Server certificate:
*  subject: C=GB; ST=London; L=London; O=Alros; OU=IT Department; CN=localhost
*  start date: Mar 13 18:51:40 2022 GMT
*  expire date: Mar 13 18:51:40 2023 GMT
*  issuer: C=GB; ST=London; L=London; O=Alros; OU=IT Department; CN=localhost
*  SSL certificate verify result: self signed certificate (18), continuing anyway.
> GET / HTTP/1.1
> Host: localhost
> User-Agent: curl/7.64.1
> Accept: */*
> 
< HTTP/1.1 200 OK
< Server: nginx/1.21.6
< Date: Sun, 13 Mar 2022 18:53:47 GMT
< Content-Type: text/html; charset=utf-8
< Content-Length: 12
< Connection: keep-alive
< 
* Connection #0 to host localhost left intact
hello world!* Closing connection 0
```

# 密码

完整代码可在 [GitHub](https://github.com/alros/docker-nginx-flask-demo) 上获得。