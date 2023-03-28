# 像专家一样调试本地应用程序

> 原文：<https://medium.com/geekculture/debug-react-native-apps-like-a-pro-11040aeccdba?source=collection_archive---------2----------------------->

调试崩溃的 Android 应用程序的小步骤，没有错误日志

![](img/9c6bc053ececba09abd00b88b8a13bdd.png)

作为一名移动开发人员，你的应用偶尔会崩溃或冻结，而没有任何具体原因，这并不罕见。调试**。apk** 文件会导致许多令人头疼的问题，因为您没有连接远程调试器，有时根本没有错误日志。

## Android 调试桥

ADB 是一个命令行工具，它为开发人员提供了与设备通信的另一种方式。它为我们提供了一种通过终端安装和调试应用程序的方法。

它由客户端-服务器架构组成，包含三个主要组件:

*   **客户端**:运行在你的机器上，发送命令
*   **守护进程** ( **adbd** )在智能手机设备上运行命令
*   **服务器**控制 **adbd** 和**客户端**之间的通信

> adb 预先包含在 Android 的 SDK 平台工具包中。

# 装置

以防你出车祸。没有显示任何错误的 apk 文件，可以使用**ADB**命令通过终端安装 app。

1.  确保您的模拟器已启动并运行。
2.  导航到与您的相同的文件夹。apk 文件并执行命令:

```
adb install myAppName.apk
```

如果你的应用有任何错误，你现在就可以看到。

![](img/416d825c056331608a7c4a32b524efc4.png)

results: app not signed properly

# 再装

如果你的应用程序死机，物理震动设备无法工作，有一个命令可以帮助你从命令行重新加载应用程序。

```
adb shell input text "RR"
```

ADB 为我们提供了对 Unix 外壳的访问，该外壳可以在设备上运行各种命令。该命令告诉设备键入“RR”，这是用于**重新加载**的 React 本地命令。

这些是最常见和最有用的 ADB 命令，但是还有许多其他命令可以用来提高 Android 开发水平。

[https://developer.android.com/studio/command-line/adb](https://developer.android.com/studio/command-line/adb)