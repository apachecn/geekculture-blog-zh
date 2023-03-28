# 如何在 Windows、macOS 和 Ubuntu 上安装 Azure CLI

> 原文：<https://medium.com/geekculture/how-to-install-azure-cli-on-windows-macos-and-ubuntu-61f1147c590c?source=collection_archive---------28----------------------->

![](img/4ba1cbe046d217dad1acc56bc824c2dc.png)

Photo by [Gabriel Heinzer](https://unsplash.com/@6heinz3r?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 前言

在过去十年的大部分时间里，云计算一直是科技行业的热门词汇之一。随着互联网的不断发展，对更多资源的需求也在不断增长，以满足其用户的存储、计算和网络需求。

一些大型科技公司将其庞大的资源池“出租”给个人和公司，因此他们不需要费心在本地拥有和维护服务器和存储资源。这些科技公司之一是微软，给了我们 T2 Azure。

M 在 [Azure](http://portal.azure.com) 门户上移动来创建、管理和执行我们的资源上的管理动作可能会很紧张和浪费时间，尤其是当涉及许多点击的时候。说实话，我们最好在我们的终端上键入单个命令，让我们的终端为我们做繁重的工作——这就是 Azure 命令行界面(CLI)的用武之地。

> Azure CLI 是官方的跨平台命令行工具，用于对 Azure 资源执行管理操作。

一旦你在你的工作站上安装了 Azure CLI，以前需要几次滚动和点击的操作将通过一个命令来完成。例如:

```
# To create a Azure Directory Identity
az ad sp create-for-rbac -n APP_NAME --skip-assignment# To configure a Web Application's 
az webapp config set --name MY_APP_NAME ...
```

# 在 Windows 上安装 Azure CLI

Windows 上的 Azure CLI 作为微软安装程序(。MSI)可分配。因此，您可以执行以下任一操作:

## 从可分发的. MSI 安装

下载 [**。MSI distributable**](https://aka.ms/installazurecliwindows) 并在 PC 上运行安装程序。这是一个非常简单的安装，按照提示。

## 通过 PowerShell 安装

要通过 PowerShell 终端安装 Azure CLI，

1.  打开提升的 PowerShell 会话(以管理员身份运行 PowerShell)。
2.  运行以下命令下载并安装(或更新，如果您已安装)最新版本的 Azure CLI 工具:

```
curl -sL [https://aka.ms/InstallAzureCLIDeb](https://aka.ms/InstallAzureCLIDeb) | sudo bashProgressPreference = 'SilentlyContinue'; Invoke-WebRequest -Uri [https://aka.ms/installazurecliwindows](https://aka.ms/installazurecliwindows) -OutFile .\AzureCLI.msi; Start-Process msiexec.exe -Wait -ArgumentList '/I AzureCLI.msi /quiet'; rm .\AzureCLI.msi
```

# 在 macOS 上安装 Azure CLI

如果你在 macOS 电脑上工作，通过 macOS 包管理器 [**自制**](https://brew.sh/) 安装 Azure CLI。

## **安装自制软件(如果您当前没有安装自制软件，则为可选)**

要安装 homebrew，请在 Mac 上打开终端会话，并运行以下命令:

```
/bin/bash -c "$(curl -fsSL [https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh](https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh))"
```

## 为 Homebrew 设置 Python3(如果没有正确设置 Python3，则为可选)

家酿需要 Python3 才能正常工作。运行以下命令以确保万无一失。

```
brew update && brew install python3 && brew upgrade python3
brew link --overwrite python3
```

## **安装 Azure CLI**

运行以下命令在您的电脑上安装 Azure CLI

```
brew update && brew install azure-cli
```

# 在 Ubuntu 上安装 Azure CLI

对于 Ubuntu 来说，值得庆幸的是，负责 Azure CLI 的官方团队创建了一个将自动完成安装过程的脚本。在终端会话中运行以下命令，您就完成了设置！

```
curl -sL [https://aka.ms/InstallAzureCLIDeb](https://aka.ms/InstallAzureCLIDeb) | sudo bash
```

就是这样，你已经准备好对你的 Azure 资源采取行动，尽可能以最有成效的方式。

分享，喜欢和**订阅下面的**，如果你觉得这篇文章有帮助！

[](https://tomisinabiodun.azurewebsites.net/subscribe) [## 订阅每周文章和编码技巧

### Azure Geek | Web 开发人员|应用程序开发人员|软件工程师

tomisinabiodun.azurewebsites.net](https://tomisinabiodun.azurewebsites.net/subscribe)