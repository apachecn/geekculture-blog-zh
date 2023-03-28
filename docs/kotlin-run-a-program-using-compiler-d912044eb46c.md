# 科特林:使用编译器运行程序

> 原文：<https://medium.com/geekculture/kotlin-run-a-program-using-compiler-d912044eb46c?source=collection_archive---------18----------------------->

![](img/0df9512c50c69c1931d90d50f4ab7eb8.png)

Photo by [Hope House Press - Leather Diary Studio](https://unsplash.com/@hope_house_press_leather_diary_studio?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在本文中，我将向您介绍使用 Kotlin 编译器运行 Kotlin 程序的步骤。

我首先简单回答一些基本问题。

***什么是编译器？***

编译器是将人类可读的代码(源代码)转换成机器可理解的代码(字节码)的程序。

***科特林的编译器是什么？***

> Kotlin 编译器的名字是 kotlinc。

Kotlin 编译器是如何工作的？

类似于 Java，Java 编译器(javac)将 Java 源代码编译成字节码(。类)，Kotlin 编译器(kotlinc)获取 Kotlin 源代码并将其编译成字节码(。类)，字节码运行在 Java 虚拟机(JVM)上。

以防 Kotlin 文件中有任何 Java 代码(。kt)，它将由 JVM 编译。

> Kotlin 与 Java 100%互操作

要开始，您需要安装 Kotlin 命令行编译器。每一个 Kotlin 版本都附带了一个独立版本的编译器。可以从[***GitHub***](https://github.com/JetBrains/kotlin/releases/tag/v1.5.0)***下载最新版本。***

***手动安装***

将上面链接中的独立编译器解压到一个目录中，并可选地将 bin 目录添加到系统路径中。bin 目录包含在 Windows、OS X 和 Linux 上编译和运行 Kotlin 所需的脚本。

***自制***

或者，在 OS X 上你可以安装编译器 [***【家酿***](https://brew.sh/) 。

```
$ brew update
$ brew install kotlin
```

***抓包***

如果使用***Snap-on***Ubuntu 16.04 或更高版本，可以从命令行安装编译器。

```
$ sudo snap install --classic kotlin
```

这就是设置。

***创建并运行一个应用***

用 Kotlin 创建一个简单的应用程序，显示“Hello，Kotlin！”。在您最喜欢的编辑器中，用下面几行创建一个新文件 *Hello.kt* :

```
fun main() {
    *println*("Hello, Kotlin!")
}
```

***使用 Kotlin 编译器编译应用程序。***

打开终端，进入保存 Kotlin 程序的目录。以下命令将 Kotlin 程序转换成 jar 文件。

```
$ kotlinc Hello.kt -include-runtime -d Hello.jar
```

**解释:**

*   ***-include-runtime:****默认情况下，Kotlin 编译为 Java。它需要 Java 库在运行时运行。因此，要在运行时包含 Kotlin 所需的库，您需要编写以下代码。*
*   ****-d Hello.jar:*** 将创建一个 jar 文件，文件名为。*

*要执行生成的 jar 文件，请运行以下命令。*

```
*java -jar Hello.jar*
```

*就是这样，你会看到“你好，科特林！”打印在控制台中。*

*我将维护一个用 Kotlin 编写的 DS /算法的 [***Github 库***](https://github.com/aroranubhav/KotlinDSAlgorithms) 。看看吧！*