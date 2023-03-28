# RStudio 服务器、Linux(Ubuntu)、WSL(Linux 的 Windows 子系统)和 Docker

> 原文：<https://medium.com/geekculture/rstudio-server-linux-ubuntu-wsl-windows-subsystem-for-linux-and-docker-ac0a855ed20a?source=collection_archive---------29----------------------->

如果我想使用 RStudio 服务器，我需要 Linux 作为操作系统。像我这样的 Windows 用户需要为 RStudio Server 设置 **WSL** (Windows 子系统 for Linux)。首先，确定你的 Windows 是否是最新版本。

1.  设置 **WSL**

在 Windows 搜索中查找**打开或关闭 Windows 功能**。

检查`Virtual Machine Platform`和`Windows Subsystem for Linux`。然后重启你的设备。

2.将 WSL 版本更改为 **2**

```
# In the Windows PowerShell as Administratorwsl --set-default-version 2 # See https://aka.ms/wsl2kernel if you have an issue
```

3.安装 **Ubuntu**

在微软商店找到在 Windows 上运行 Linux 的**。点击下载 **Ubuntu****

**4.设置 **Ubuntu 账户****

**运行你已经安装的 Ubuntu，输入你想要的用户名和密码。这些用户名和密码将在 RStudio 服务器中使用。**

**5.更新 Ubuntu**

```
# In the Ubuntusudo apt update
sudo apt upgrade -y# You cannot use ctrl+V in the Ubuntu. Instead Use the right click
```

**6.安装最新的 R 和 RStudio 服务器**

```
# See https://cran.r-project.org/bin/linux/ubuntu/ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E298A3A825C0D65DFD57CBB651716619E084DAB9sudo add-apt-repository 'deb https://cloud.r-project.org/bin/linux/ubuntu focal-cran40/' sudo apt updatesudo apt install -y r-base r-base-core r-recommended r-base-dev gdebi-core build-essential libcurl4-gnutls-dev libxml2-dev libssl-dev ## Install RStudio Server ##wget https://rstudio.org/download/latest/stable/server/bionic/rstudio-server-latest-amd64.debsudo gdebi rstudio-server-latest-amd64.deb
```

**7.运行 RStudio 服务器**

```
sudo rstudio-server start
```

**在网络浏览器中，`http://localhost:8787`，输入你已经为 Ubuntu 创建的用户名和密码。**

**8.提示并关闭 RStudio 服务器**

```
# Some packages may cause an install issue. You may need an dependency. 
# https://jeroen.cran.dev/V8/sudo apt-get install -y libv8-dev# Then install R package V8## To Close RStudio Server ##sudo rstudio-server stop
```

**9.码头工人**

**现在您可以使用 RStudio 服务器了。但是每次都需要打开 Ubuntu，输入代码。当你关闭它的时候，你也需要在 Ubuntu 中输入代码。此外，当您需要使用 R 运行多台机器时，您可能想要高效的东西。 **Docker** 可以是一个答案。下载 [Docker](https://www.docker.com/) 。**

**运行 Docker。您可以在左侧面板中找到**图像**选项卡。Docker 需要一个图像来运行，而不是文件。我们需要 Rstudio 图像。**

**转到[按钮](https://hub.docker.com/)。然后在搜索中输入`rocker`。你可以找到`rocker/rstudio`。复制`docker pull rocker/rstudio`，然后粘贴到 Powershell 中。**

**你可以在 docker 的**图片**标签中找到`rocker/rstudio`。**

**点击**运行**即可。在选项中，选择**容器名称**。比如“rstudio-1”。然后像 1001 一样配置 4 位数的**本地主机**。**

**现在你可以在**容器/应用**标签中看到`rstudio-1`。在网络浏览器中，键入`http://localhost:1001/`。ID 和密码都是由图像创建者创建的 rstudio。**

**10.更新项目的图像**

**使用摇杆的图像创建 R 环境后，您需要创建自己的图像。**

```
# In the Powershell, check docker containers 
docker ps -adocker commit {Container ID} {New image name}
#ex# docker commit 40b8d16b7dba rstudio-2
```

**现在你可以在**图像**标签中找到`rstudio-2`。这个`rstudio-2`与您已经创建的环境相同。不需要在 Ubuntu 中输入代码就可以运行这个。如果你制作多个容器，你可以同时运行多个 rs。**

**最初发布在[我的网站](https://youngjoon5.github.io/)**