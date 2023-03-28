# 如何在联想军团 Y530–15ICH 双开机上安装 Linux

> 原文：<https://medium.com/geekculture/how-to-install-linux-on-lenovo-legion-y530-15ich-dual-boot-9957c335c21c?source=collection_archive---------13----------------------->

![](img/63a2c4ccc96f2395db6ce5d81c2dc221.png)

联想军团 Y530 是一款出色的游戏和开发笔记本电脑，但有时如果你不喜欢 windows，你可以更换它，但要小心，因为我试图安装 Ubuntu，但我失败了，因为触摸板和键盘不起作用，我尝试了另一个发行版。

# 流行操作系统

Pop OS 对于开发、游戏甚至设计来说都是一个非常棒的 Linux 发行版，它支持 Nvidia，你可以定制它。我搜索了一下，我真的很喜欢它，最好的部分是，在军团 Y530 工程真的很顺利，没有问题，你可以尝试一下。

进入主页面 [Pop OS 的页面](https://pop.system76.com/)点击下载，搜索最适合您系统的 Pop OS 版本

![](img/6e42ec39722a7b23e8817b949a161ea7.png)

Pop OS Page

![](img/47449f2374c75b3f3b4f3f7dbf7f04b0.png)

Pop OS download ISO

# 可引导 USB

你需要[以太程序](https://www.balena.io/etcher/)和你的 Pop OS ISO 文件

![](img/909a60e0fcf9f81f7c96a0c9348b8b4d.png)

Etcher program to flash or boot ISO

# Windows 分区

你需要按下视窗按钮，然后点击鼠标按钮右边的视窗标志

![](img/163df0316cababeec785f34093f90056.png)

Windows Logo

然后，您需要单击磁盘管理

![](img/63d6aebcb88a26f6fcc6d972b06f0508.png)

Disk Management example

![](img/391f00d7aa45a09e6434f349911d3187.png)

Shrink option

将您的磁盘空间缩小到您想要安装 Pop OS 的空间

![](img/23d962cd4ba68763c3fc449a026385c9.png)

Shrink Disk

![](img/bb3eae57b3f72998242e00738c7b8049.png)

Example Shrink complete

# 从 USB 重启和引导

在 BIOS 设置中的安全>安全启动>禁用中禁用启动安全

![](img/237c7eaa0d1ca58c9d52f7d321ba9c85.png)

Example Disabled boot secure

在军团 Y530–15ICH 中，您需要打开并按 f12，它将打开引导菜单选择您的 USB，并将打开 Pop OS 的安装。

选择您的语言

![](img/43ed9698748bbac6755f440bf68355f1.png)

Select language Pop OS

选择键盘语言

![](img/87a75b087b7c232db376cbfba1cd67be.png)

Keyboard Language

选择自定义，因为我们将与 Windows 10 一起创建双启动

![](img/1fdd2e00d8d05501490f331a69c6cbc7.png)

Custom partition

它将打开 Gparted，我们需要选择未分配的分区并创建三个分区 fat32、linux-swap 和 ext4

![](img/fb66440dcc28ac5cf5a6428c3224336b.png)

Selecting unallocated partition

fat32 分区为 512mb，但现在建议为 1 GB

![](img/d7f7d453f20a85821f5b9917aa3d0db3.png)

fat32 partition

然后，我们需要再次选择未分配的分区，并创建 4096mb 的 linux-swap

![](img/e5bf740574622e33e93a11b43f3fcbb6.png)

linux-swap partition 4096mb

最后一次，我们再次选择未分配的分区，我们需要格式化为 ext4

![](img/100c984a6280a43112cdf507e99de871.png)

Unallocated partition to ext4

我们需要单击“apply ”,如果一切正常，我们可以关闭窗口，将每个分区用于 Pop OS。

![](img/d6f11f21d91dd3376a1a3fc3244add83.png)

Apply button

![](img/ea49ca28af0079bae43256bc39f6b692.png)

Using fat32 partition

![](img/54f4615a25633a46f7d9689bd03b2d1a.png)

Using linux-swap partition 4096

![](img/fcf43a3b992d89c59cf1cb52e8cad90c.png)

Using the final partition, ext4

![](img/a5e457ac4b60768418c9bc168538df6f.png)

The erase button is enabled

![](img/d559f22f6031fd59ab11e456962fb113.png)

Restart the device and check your dual boot

# 双靴

你需要选择一个或另一个，打开你的笔记本电脑，按 f2 或 f12，它会显示你的启动管理器

![](img/ef83e753256f329e8e426a3dc3081877.png)

# 或者您可以添加操作系统探测器

# 1.更新您的系统

打开终端并运行以下命令:

`sudo apt update sudo apt upgrade`

# 2.安装操作系统探测器

现在我们将安装一个工具来帮助 Pop！_OS 识别 Windows 分区。

运行以下命令:

`sudo apt install os-prober sudo os-prober`

你应该会看到类似/dev/sda#的东西:Windows 10:Windows；链条

运行以下命令，以便 GRUB 可以使用新的分区数据进行更新:

`sudo update-grub`

# 安装 Pop OS 后要做的事情

# 1.更新和升级

```
sudo apt update
sudo apt full-upgrade
```

# 2.启用最小化和最大化按钮

```
sudo apt install gnome-tweaks
```

![](img/9cdf9f05f722243df8eb67b311702c3a.png)

Enabled and Maximize window title bars

# 3.将仪表板安装到坞站

**这一步在 Pop 中是可选的！_OS 版本 21.04 或更高版本，因为他们添加了原生 dock，您根本不需要添加 dash dock，只需进入设置>桌面> Dock，您只能在 21.04 版本上自定义它，如果您有 21.04 或更高版本，您可以跳过这一步**

[Dock link 火狐](https://extensions.gnome.org/extension/307/dash-to-dock/)

![](img/a6264bc0e92db5ee25bf930adbbe7878.png)

Click browser dock extension

刷新页面，您将看到切换

![](img/06c8061f371047ada014b3be369854e9.png)

Toggle button

# 4.应用程序

![](img/02df0049f0e4f9e311d36aa82a4dd4e7.png)

install all the app that you wanted

# 5.安装受限格式

非常有用，因为它添加了其他流行的媒体编解码器

```
sudo apt-get install ubuntu-restricted-extras
```

![](img/6a6ebc04d20b762f9972d8260abc70ae.png)

restricted formats

# 6.使用 tlp 延长电池寿命

```
apt-get install tlp tlp-rdwtlp start
```

# 7.安装 flatpak

```
sudo apt install flatpak
sudo apt install gnome-software-plugin-flatpak
```

# 8.安装搜索以更改下载的服务器

```
sudo apt search software-properties
sudo apt install software-properties-gtk
```

![](img/34aecdd5c95c84bc24a13eb412034de1.png)

Software updates

![](img/d19e14a97361fb62c9e7e2098e7c2dc9.png)

Example Software updates configuration

# 9.安装快照和快照存储

```
sudo apt update
sudo apt install snapd
sudo snap install snap-store
```

![](img/74e4ef5fd6922da944aed0b5779232db.png)

# 10.安装 AppImageLuncher

[](https://github.com/TheAssassin/AppImageLauncher/releases) [## 释放 Assassin/AppImageLauncher

### 此时您不能执行该操作。您已使用另一个标签页或窗口登录。您已在另一个选项卡中注销，或者…

github.com](https://github.com/TheAssassin/AppImageLauncher/releases) 

有了它，你可以为这个库或其他库安装任何 AppImage，要安装它，你需要搜索 de。deb 并下载它，然后只需点击两次，直到弹出显示并安装它

![](img/09666173d872d9e7cbdcdd54896261c4.png)

Example installing AppImageLuncher

 [## AppImage 格式的应用程序

### 适用于 Linux 的 AppImage 应用程序，无需安装

appimage.github.io](https://appimage.github.io/apps/) 

# 11.安装 time Machine 进行备份

```
sudo apt-add-repository -y ppa:teejee2008/ppa
sudo apt install timeshift
```

![](img/1ea4db00e9eed4d96c5b85ea9199ecc3.png)

Install timeshift for backups

# 结论

这个操作系统很迷人，它有很多软件和对开发和游戏的支持，它真的很容易使用，最棒的是它可以完美地与军团 Y530–15ICH 配合工作。军团 Y530–15ICH 工作流畅，非常安静。我真的很喜欢这种组合，我当然推荐这个操作系统！！！。

# 来源

[1](https://www.youtube.com/watch?v=EXZ7_DVxztQ) ， [2](https://www.youtube.com/watch?v=CozK7sJ8UMs) ， [3](https://www.youtube.com/watch?v=LHj2ulIm7AQ) ， [4](https://www.youtube.com/watch?v=J8CqMNCMCT8) ， [5](https://techhut.tv/5-things-to-do-after-installing-pop-os/) ， [6](https://pop.system76.com/) ， [7](https://www.balena.io/etcher/) ， [8](https://www.techhut.tv/dual-boot-windows-10-pop-os/)