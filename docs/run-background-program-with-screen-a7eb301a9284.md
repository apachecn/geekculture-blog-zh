# 用屏幕运行后台程序

> 原文：<https://medium.com/geekculture/run-background-program-with-screen-a7eb301a9284?source=collection_archive---------0----------------------->

![](img/fce78ac63161c6d0a3c278630b60b01e.png)

screen image is taken from google

我们经常需要 SSH 或者 telnet 远程登录 Linux 服务器，经常会运行一些需要很长时间才能完成的任务。在此期间，我们不能关闭窗口或断开连接，否则，任务将被杀死，一切都将中止或，网络将突然中断，任务没有完成。这时候我们可以打开屏幕会话来解决。

screen 命令可以将当前窗口与任务分开。即使我们离线，服务器仍然在后台运行任务。当我们再次登录到服务器时，我们可以读取窗口线程并重新连接到任务窗口。

屏幕是一个全屏窗口管理器，可以在多个线程之间复用一个物理终端。屏幕上有一个会话的概念。用户可以在一个屏幕会话中创建多个屏幕窗口。每个屏幕窗口就像操作一个真正的 SSH 连接窗口。

**安装 GNU 屏幕**

GNU Screen 在大多数 Linux 操作系统的默认存储库中都有。

```
# To install GNU Screen on Arch Linux, run:$ sudo pacman -S screen# On Debian, Ubuntu, Linux Mint:$ sudo apt-get install screen# On Fedora:$ sudo dnf install screen# On RHEL, CentOS:$ sudo yum install screen# On SUSE/openSUSE:$ sudo zypper install screen
```

# 主要功能

*   **会话恢复:**只要屏幕本身没有终止，就可以恢复在其中运行的会话。这对远程登录的用户特别有用——即使网络连接中断，用户也不会失去对已经打开的命令行会话的控制。只需再次登录主机并执行 **screen -r** 即可恢复会话。同样，在暂时离开的时候，也可以执行 detach 命令来暂停屏幕(切换到后台)，同时保证里面程序的正常运行。这非常类似于图形界面下的 VNC。

```
# To start the session
$ screen -S screen_name# To resume the session or Reattach session enters a screen process
$ screen -r# If there will be multiple screen running then 
$ screen -r screen_name# To detach the session
$ screen –d screen_name# View all screen sessions
$ screen -ls
```

进入屏幕会话后，您可以在会话中创建多个窗口并管理这些窗口。管理命令以 **ctrl+ a** 开始

```
ctrl + a + c: create a new window (create)ctrl + a + n: switch to the next window (next)ctrl + a + p: switch to the previous window (previous)ctrl + a + w: list all windowsctrl + a + A: Rename windowctrl + a + d: detach the current sessionctrl + a + [1-9]: switch to the specified window (1-9 is the window number)ctrl + d: Exit (close) the current window
```

*   **多窗口:**在屏幕环境下，所有的会话都是独立运行的，都有自己的编号、输入、输出和窗口缓存。用户可以通过快捷键在不同窗口之间切换，可以自由重定向每个窗口的输入输出。Screen 实现基本的文本操作，如复制和粘贴等。它还提供了类似滚动条的功能来查看窗口状态的历史记录。还可以对窗口进行分区和命名，并且可以监视后台窗口的活动。
*   **会话共享:**屏幕允许一个或多个用户从不同的终端多次登录一个会话，并共享该会话的所有特征(例如，您可以看到完全相同的输出)。它还提供了一种窗口访问权限机制，可以对窗口进行密码保护。

# 新窗口

创建新窗口有三种方式:

**$ screen**#这样，你可以创建一个新的窗口并进入一个窗口，但是这样一来这个窗口就没有名字了，并且无法区分它们

**$ screen-S name**#这将创建一个名为 name 的新窗口，并将其合并到窗口中

> 例如，screen -S count 创建了一个名为 count 的新窗口，并输入

**$ screen 命令**#创建一个新窗口并在窗口中执行命令，同样没有名字

> 例如 screen python。/sample.py 创建并执行 sample.py 程序

# 其他有用的屏幕命令

```
# Terminate Screen session, If the session is no longer needed, just kill it. To kill the disconnected session named senthil:screen -r sample -X quitor:screen -X -S sample quitor:screen -X -S 29415 quit
********************************************************************Ctrl + a :list all conversationsCtrl + a 0 :switch to session number 0Ctrl + an :switch to the next sessionCtrl + ap :switch to the previous sessionCtrl + a S :Split the current area into two areas horizontallyCtrl + al :Split the current area into two areas verticallyCtrl + a Q :Close all sessions except the current sessionCtrl + a X :close the current sessionCtrl + a \ :Terminate all sessions and terminate ScreenCtrl + a? :Show key bindings. To log out, press enter #### Lock session
******************************************************************# Screen has an option to lock the session. To do this, press Ctrl + a and x. Enter your Linux password to lock it.Screen used by sk on ubuntuserver.Password:# Record sessionYou may want to record everything in the Screen session. To do this, just press Ctrl + a and H.# Alternatively, you can use the -L parameter to start a new session to enable logging.screen -LFrom now on, all the activities you do in the session will be recorded and stored in a file named screenlog.x in the $HOME directory. Here, x is a number.You can use the cat command or any text viewer to view the contents of the log file.
```

## **关闭会话窗口**

如果你想关闭一个额外的窗口，有三种方法:

> **kill -9 threadnum** 比如上面的 2637，kill -9 2637 可以杀死线程，当然也杀死了窗口
> 
> 使用 **Ctrl a +k** 终止当前窗口和窗口中运行的程序
> 
> 使用 **Ctrl a 并输入退出命令**退出屏幕会话。需要注意的是，这个退出会杀死所有的窗口，并退出所有正在运行的程序

## 清理死窗

当窗口被杀死时，可以使用 **screen -ls** 查看(？？？dead)在窗户后面，表示窗户是死的，但是仍然占用空间。这时你需要清理窗户

> $ screen-wipe #自动清除死窗口

这种窗明几净~

如果没有打开的会话，您将看到以下输出:

> $ screen -ls
> 
> 在/run/screens/S-sk 中没有找到套接字。

有关更多详细信息，请参考手册页:

> $ man 屏幕

我希望你喜欢这个小会议。如果您有任何意见或问题或建议，请加入下面的论坛讨论！

感谢支持( [***跟着***](/@budhdisharma) 或者[***Clapp***](https://medium.com/p/761e6da812b8/edit))！！！！！

[](https://budhdisharma.medium.com/) [## 布迪夏尔马-中等

### 在这个博客中，我们将讨论 Android 的 MVP 设计模式，它是一个更好的和改进的替代方案…

budhdisharma.medium.com](https://budhdisharma.medium.com/)