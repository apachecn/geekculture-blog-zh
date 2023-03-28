# Unix 文件系统解释

> 原文：<https://medium.com/geekculture/unix-file-system-explained-9d3554de9d46?source=collection_archive---------2----------------------->

## 作为 Windows 用户解释 Linux 和 Android 文件系统

![](img/8beef23e8d377a75319c0aaf53a9cb4d.png)

aodba.com

根据 W3Tech 的数据，大约 80%的网络服务器使用 Linux 衍生操作系统。另一个 Linux 衍生操作系统 Android 占据了移动操作系统市场约 [73%的份额。你可能使用过 Linux，但是不知道你是否使用过 Windows 作为桌面操作系统。下面是向 Windows 用户解释的 Linux 文件系统。](https://www.statista.com/topics/876/android/)

## 什么是 Linux？

Unix 是 20 世纪 60 年代末开发的计算机操作系统。Unix 最初的设计是作为一个用于研究目的的开源、多用户、多任务操作系统。Unix 操作系统已经成为个人计算机和大多数大型计算系统的标准软件环境之一。

[在这里注册我的电子邮件列表。](/subscribe/@drechang)

## Unix 和 Windows 如何存储系统二进制文件和库？

Windows 将系统库和二进制文件存储在这些**C:\ Windows \ System32**&**C:\ Windows \ sys wow 64**中。另一方面，Linux 在以下文件夹中安装二进制文件和库。

**/bin**

这是操作系统存储操作系统的执行二进制文件的地方。shell、GNU 实用程序等二进制文件存储在这个目录中。

因此，您会发现诸如以下命令

*   丙酸纤维素
*   增加
*   空间

**/lib**

库是一个包含 API 的文件，供二进制文件访问。所以这个文件夹包含了所有的系统库。

**/sbin**

系统管理二进制文件存储在这里。

您会发现诸如以下命令

*   lsmod
*   ifconfig
*   关机

在这里注册我的电子邮件列表。

## Windows 和 Unix 如何处理用户文件？

在 Windows 上，你有一个叫做 **C:\Users** 的用户文件夹，所有的用户都把他们的文件存放在那里。另一方面，Linux 将其用户文件存储在以下文件夹中。

**/根**

这里是根用户存储他们所有文件的地方。

**/首页**

除了根用户之外的所有用户都将他们的文件存储在这个位置。

在这里注册我的电子邮件列表。

## Windows 和 Unix 如何处理进程？

Windows 不将它们的进程存储在文件夹中。所以你必须进入**任务管理器**来获取相关信息。

**/进程**

在 Unix 中一切都是一个文件。您可以访问这个文件夹来查看 Unix 中所有正在运行的进程，而不是使用任务管理器来查看您的进程。现在如果你想得到一个花哨的界面来查看信息，你可以使用[最上面的](https://en.wikipedia.org/wiki/Top_%28software%29)来给你更多的信息。

## Windows 和 Unix 如何处理设备管理？

Windows 使用**设备管理器**来管理他们的设备，比如闪存、打印机等。另一方面，Linux 将每个设备存储在一个文件中。

**/开发**

与 Windows 不同，Unix 将设备存储为文件。大多数 USB 设备和硬盘都有以下命名约定。

```
/dev/sd*
```

星号可以是一个字母或数字，用来标记所连接的每台设备。

在这里注册我的电子邮件列表。

## Windows 和 Unix 如何处理挂载的设备？

对于您连接到设备的每个存储媒体，Windows 将从 C:\到 Z:\自动装入设备，前提是它具有兼容的存储类型和文件系统。另一方面，Linux 要求您将设备挂载到下面的文件夹中。

**/mnt**

这是系统或用户将安装其设备以访问其存储设备上的文件的位置。

## Windows 和 Unix 如何处理临时文件？

Windows 将每个临时文件存储在 **C:\Windows\Temp** 中。另一方面，Unix 将临时文件存储在以下文件夹中。

**/tmp**

系统将自动存储程序自动创建的所有临时文件。所以大部分缓存和日志记录可能都存储在这里。

在这里注册我的电子邮件列表。

## Windows 和 Unix 如何处理系统启动？

Windows 已经内置了一个引导加载程序，可以自动加载操作系统。假设您正在为您的 Unix 系统使用诸如 [GNU GRUB](https://www.gnu.org/software/grub/) 之类的引导加载程序，引导加载程序必须读取一个文件夹来引导操作系统。

**/开机**

实际的引导加载程序而不是操作系统将使用这里的文件来决定如何启动整个操作系统。

在这里注册我的电子邮件列表。

## Windows 和 Unix 如何处理程序配置？

未设置 Windows 的配置文件存储。所以你可以在 **C:\Program Files，C:\Program Data，AppData** 等里面找到。

**/等等**

该文件夹包含 Linux 系统上每个程序的所有系统级配置。

在这里注册我的电子邮件列表。

## Windows 和 Unix 如何处理用户程序二进制、库和数据？

Windows 中每隔一个程序二进制文件和私有库存储在 **C:\Program Files** 和 **C:\Program Files(x86)** 中。程序数据将存储在 **C:\Program Data** 和 AppData 文件夹**中。** Unix 将其所有用户程序二进制文件、库和数据存储在以下文件夹中。

**/usr**

这个目录通常包含二进制、库、数据等文件夹的版本。通常，用户安装的应用程序会将其文件存储在该目录中。

在这里注册我的电子邮件列表。

## Windows 和 Unix 如何处理外来文件？

Windows 没有设置位置来设置无关文件。Unix 通常将无关文件存储在以下文件夹中。

**/var**

这是程序存储大量相同类型数据的地方。例如，大多数电子邮件服务将他们的电子邮件存储在**/var/mail**文件夹中。

在这里注册我的电子邮件列表。

**相关内容:**

*   [如何在 Linux/BSD 中实现任务自动化？](/geekculture/how-to-automate-tasks-in-linux-bsd-91d0b0560f5)
*   [如何在 Linux 上安全删除文件？](/geekculture/how-to-securely-delete-files-in-linux-ce6ad1205922)
*   [快速简单的安装 Arch Linux 的方法](/geekculture/the-quick-and-easy-way-to-install-arch-linux-70b9bfc35863)