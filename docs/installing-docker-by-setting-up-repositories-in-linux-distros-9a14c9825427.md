# 通过在 Linux 发行版中设置存储库来安装 Docker

> 原文：<https://medium.com/geekculture/installing-docker-by-setting-up-repositories-in-linux-distros-9a14c9825427?source=collection_archive---------3----------------------->

在新的主机上第一次开始安装 Docker 引擎之前，您需要设置 Docker 存储库。稍后，您可以从存储库中安装和更新 Docker。

> **T3 安装在 CentOST5 上**
> 
> *设置仓库，安装 Docker 引擎。
> 安装* `*yum-utils*` *包(它提供了* `*yum-config-manager*` *实用程序)并建立稳定的库。*

```
sudo yum install -y yum-utils

sudo yum-config-manager \
    --add-repo \…
```