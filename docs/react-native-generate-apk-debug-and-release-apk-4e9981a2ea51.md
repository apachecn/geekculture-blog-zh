# React 本机生成 APK —调试并发布 APK

> 原文：<https://medium.com/geekculture/react-native-generate-apk-debug-and-release-apk-4e9981a2ea51?source=collection_archive---------0----------------------->

![](img/81e8b2c1506d44565664603c79833688.png)

> **什么是安。apk 文件？**

Android 软件包(APK)是 Android 操作系统用于分发和安装移动应用程序的软件包文件格式。它类似于。. apk 文件适用于 android 系统。

# 调试 APK

## 我能用它做什么？

调试。apk 文件将允许您在发布到应用商店之前安装和测试您的应用。请注意，这还没有准备好发布，在发布之前您还需要做很多事情。尽管如此，它对于最初的发行和测试还是很有用的。

您需要在手机上启用调试选项来运行此 apk。

## 先决条件:

*   react-原生版本> 0.58

## 如何 3 步生成一个？

*步骤 1:* 在终端中转到项目的根目录，运行下面的命令:

`react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res`

*步骤 2:* 转到 android 目录:

`cd android`

*步骤 3:* 现在在这个`android`文件夹中，运行这个命令

`./gradlew assembleDebug`

那里！你会在下面的路径中找到 apk 文件:
`yourProject/android/app/build/outputs/apk/debug/app-debug.apk`

# 释放 APK

> **第一步。生成密钥库**

您将需要一个 Java 生成的签名密钥，这是一个用于为 Android 生成 React 本机可执行二进制文件的密钥库文件。您可以使用终端中的 *keytool* 命令创建一个

`keytool -genkey -v -keystore your_key_name.keystore -alias your_key_alias -keyalg RSA -keysize 2048 -validity 10000`

运行 keytool 工具后，系统会提示您输入密码。* *确保你记住了密码*

你可以把***your _ key _ name***改成你想要的任何名字，还有***your _ key _ alias***。出于安全原因，此密钥使用密钥大小 2048，而不是默认的 1024。

> **步骤二。将密钥库添加到您的项目**

首先，您需要复制文件 *your_key_name.keystore* ，并将其粘贴到 React 原生项目文件夹中的 ***android/app*** 目录下。

在终端上:

`mv my-release-key.keystore /android/app`

您需要打开您的*Android \ app \ build . gradle*文件并添加密钥库配置。使用 keystore 配置项目有两种方式。一、常见的无担保方式:

build.gradle - keystore Configuration

这不是一个好的安全做法，因为您以纯文本的形式存储密码。而不是将您的密钥库密码存储在。gradle 文件，如果您是从命令行构建的话，您可以规定构建过程来提示您输入这些密码。

要使用 Gradle 构建文件提示输入密码，请将上面的配置更改为:

Release Bundle Configuration Command

> **第三步。释放 APK 一代**

使用
`cd android`将您的终端目录放置到 *android*

对于 Windows，
`gradlew assembleRelease`

对于 Linux 和 Mac OSX:
`./gradlew assembleRelease`

结果，**APK 创建过程完成**。你可以在*Android/app/build/outputs/apk/app-release . apk*找到生成的 APK。这是真正的应用程序，你可以把它发送到你的手机上或者上传到谷歌 Play 商店。恭喜你，你刚刚生成了一个 Android 的 React 本地发布版本 APK。