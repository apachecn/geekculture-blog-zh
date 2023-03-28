# 如何在 alpine Docker 镜像中安装特定的 Node.js 版本？

> 原文：<https://medium.com/geekculture/how-to-install-a-specific-node-js-version-in-an-alpine-docker-image-3edc1c2c64be?source=collection_archive---------0----------------------->

![](img/75f0b5040b61e5099fa4ee9b14c28fab.png)

Why is this so complicated?

就像这些人问的: [1](https://stackoverflow.com/questions/45973481/alpine-linux-in-docker-container-env-cant-execute-node-no-such-file-or) 、 [2](https://stackoverflow.com/questions/62915459/add-a-specific-node-version-in-ruby-alpine-docker-image) 、 [3](https://stackoverflow.com/questions/58725215/how-to-install-nodejs-v13-0-1-in-alpine3-8) 、 [4](https://stackoverflow.com/questions/64243684/install-node-js-from-source-on-alpine) 等等。

除非您从节点映像开始，否则您可能会遇到这个令人惊讶的复杂问题。

原来， [alpine package repo](https://pkgs.alpinelinux.org/packages) 只为每个包存储一个版本。因此，如果您正在固定一个严格的节点版本(正如您应该做的那样)，很可能您在那里找不到它。更糟糕的是，如果你真的找到它并开始用这个设置开发，他们会在有新版本可用时立即删除这个包。所以你不得不更新或者寻找更好的解决方案。另见:[Docker 和 Alpine 的包销问题](https://stschindler.medium.com/the-problem-with-docker-and-alpines-package-pinning-18346593e891)

[据高山团队](https://gitlab.alpinelinux.org/alpine/abuild/-/issues/9996#note_87135):

> 官方的建议是保留您自己的镜像/存储库，其中包含您可能想要使用的所有特定包及其版本。

我想这是可行的，但是假设你没有疯狂到这样做，这里有一些其他的选择。

# 1、使用二进制

这并不像听起来那么简单。简单地使用你的平台的官方节点二进制文件[将会失败](https://stackoverflow.com/questions/45973481/alpine-linux-in-docker-container-env-cant-execute-node-no-such-file-or)，你要么安装额外的依赖项，要么自己构建二进制文件(你可以[找到](https://github.com/oznu/alpine-node)构建的二进制文件)。

这是一个流行的解决方案，但是您失去了跨平台支持。随着 ARM 平台(如 Apple silicon)的普及，应该避免这种已经次优的解决方案。

# 2、使用节点版本管理器，如 NVM

安装越来越多的工具违背了使用 Alpine 映像的目的，但无论如何，nvm 在最新的 Alpine 上对我不起作用。

# 3、从官方节点镜像复制二进制文件

这非常有效。它是跨平台的，即使下面这个幼稚的实现也不会复制太多或者覆盖任何会导致问题的东西。

```
FROM node:$NODE_VERSION-alpine AS nodeFROM baseCOPY --from=node /usr/lib /usr/lib
COPY --from=node /usr/local/share /usr/local/share
COPY --from=node /usr/local/lib /usr/local/lib
COPY --from=node /usr/local/include /usr/local/include
COPY --from=node /usr/local/bin /usr/local/bin
```

你可以看到它在这里练习，我用它来启用最后一个选项:

# 4、找到预装节点的镜像

所以您的基本映像没有节点，因为您使用的是其他框架，比如 Ruby、Python 等。但是也有可能找到安装了节点的基本 Alpine 映像。

对于 Ruby， [docker-ruby-node](https://github.com/timbru31/docker-ruby-node) 做到了这一点，但是，你不能锁定确切的节点版本，当有节点更新时，它们也会覆盖映像，这就给你留下了与 Alpine 包管理器相同的问题。

出于这个原因，我创建了 [ruby-node-alpine](https://github.com/thisismydesign/ruby-node-alpine) :一个能够为定制的特定版本构建 Ruby + Node Alpine 映像的项目。

你也可以在 [DockerHub](https://hub.docker.com/repository/docker/thisismydesign/ruby-node-alpine) 上找到这些图片:

```
docker run thisismydesign/ruby-node-alpine:3.0.2-16.13.0-alpine /bin/sh -c "ruby --version && node --version"
```

我很确定这个平庸的问题浪费了数千小时的开发时间。终于，我再也不用关心它了！🎉