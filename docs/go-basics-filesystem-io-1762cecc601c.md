# Go 基础:文件系统 IO

> 原文：<https://medium.com/geekculture/go-basics-filesystem-io-1762cecc601c?source=collection_archive---------8----------------------->

## 在 Go 中使用文件和目录的基本指南

![](img/c557a3277c9ac8f460c447b15d5fd7bc.png)

Image on [technotification.com](https://www.technotification.com/2019/02/new-popular-programming-languages.html)

# 介绍

在任何语言中，读写磁盘和导航文件系统都是必不可少的。让我们来学习如何在 Go with [os](https://golang.org/pkg/os/#pkg-overview) 中实现，这是一个让我们与操作系统功能交互的包。

# 议程

```
[**Files**](#cbc9)
   [Creating & Opening Files](#3f72)
   [Reading Files](#abbc)
   [Writing & Appending to Files](#e16c)
   [Removing Files](#0016)[**Directories**](#47ff)[Creating Directories](#e92d)
   [Navigating & Reading Directories](#e3b0)
   [Walking through a Directory](#8dc5)
```

# 文件

## 创建和打开文件

可以用`[os.Create](https://golang.org/pkg/os/#Create)`创建文件，用`[os.Open](https://golang.org/pkg/os/#Open)`打开文件。两者都接受一个文件路径，如果不成功，返回一个`[File](https://golang.org/pkg/os/#File)`结构和一个非 nill 错误。

如果您在一个已存在的文件上调用`os.Create`，它将截断该文件:该文件的数据被删除。相反，在不存在的文件上调用`os.Open`会导致错误。

如果成功，我们就可以使用返回的`File` struct 向文件中写入和读取数据(*将有一个打开文件、逐块读取它并在下一节*中关闭它的示例)。

最后，在使用`[File.Close](https://golang.org/pkg/os/#File.Close)`与返回的文件进行交互后关闭它是很好的。

## 读取文件

我们处理文件的一种方法是一次读取它的所有数据。我们可以用`[os.ReadFile](https://golang.org/pkg/os/#ReadFile)`来做到这一点。输入是文件路径，输出是文件数据的字节数组，如果不成功，则显示一个错误。

如果我们正在处理一个文本文件，我们需要将输出字节数组转换成一个字符串，以便获得文件文本。

请记住，`os.ReadFile`将读取整个文件并将其数据加载到内存中。如果文件很大，使用`os.ReadFile`会消耗大量内存。

一种内存性能方法是逐块处理文件，这可以使用`os.Open`来完成。

打开文件后，`[File.Read](https://golang.org/pkg/os/#File.Read)`被反复调用，直到 EOF(文件结束)。

`File.Read`接收一个字节数组，`b`并从文件中加载多达`len(b)`字节到`b`。然后，它返回读取的字节数，`bytesRead`，如果出错，则返回一个错误。如果`bytesRead`为 0，这意味着我们点击了 EOF，并完成了对文件的处理。

在上面的代码中，我们从文件中加载最多 10 个字节，处理这些字节，并重复这个过程直到 EOF。

对于较大的文件，这种方法消耗的内存比一次加载整个文件少得多。

## 写入和追加到文件

类似于`os.ReadFile`，还有`[os.WriteFile](https://golang.org/pkg/os/#WriteFile)`，一个将字节写入文件的函数。

关于`os.WriteFile`的注意事项

*   在将数据传递到`os.WriteFile`之前，请务必将您想要写入的数据转换到`[]byte`中。
*   如果文件还不存在，则需要使用[权限位](https://www.calleluks.com/flags-bitmasks-and-unix-file-system-permissions-in-ruby/)来创建文件。不要太担心他们。
*   如果文件路径已经存在，`os.WriteFile`将用正在写入的新数据覆盖文件中的初始数据。

`os.WriteFile`是创建一个新文件或者覆盖它的一种简洁的方法，但是当我们需要追加到一个文件的现有内容时，它就不起作用了。为了添加到文件中，我们需要利用`[os.OpenFile](https://golang.org/pkg/os/#OpenFile)`。

根据[文档](https://golang.org/pkg/os/#OpenFile) , `os.OpenFile`是`os.Open`和`os.Create`的更一般化版本。`os.Create`和`os.Open`都是内部调用。

除了文件路径，`os.OpenFile`接受一个 int `flags`和一个 int `perm`、权限位，并返回一个`File`结构。为了执行读写等操作，必须向`os.OpenFile`提供`flags`的正确组合。

Source: [https://golang.org/pkg/os/#pkg-constants](https://golang.org/pkg/os/#pkg-constants)

我们可以用一个按位 OR 将`O_APPEND`和`O_WRONLY`组合起来，并将其传递给`os.OpenFile`以获得一个`File`结构。如果我们用传入的任何数据调用`File.Write`，数据将被追加到文件的末尾。

## 删除文件

`[os.Remove](https://golang.org/pkg/os/#Remove)`获取文件或空目录的路径，并删除文件/目录。如果文件不存在，将返回一个非零错误。

既然我们已经学习了使用文件的基本知识，让我们开始使用目录。

# 目录

## 创建目录

为了创建一个新的目录，我们可以使用`[os.Mkdir](https://golang.org/pkg/os/#Mkdir)`。`os.Mkdir`接受目录名和权限位，创建一个新目录。如果函数不能创建目录，将返回一个非零错误。

在某些情况下，我们可能需要仅在程序执行期间存在的临时目录。`[os.MkdirTemp](https://golang.org/pkg/os/#MkdirTemp)`可以用来制作这样的目录。

`os.MkdirTemp`确保创建的临时目录具有唯一的名称，即使被多个 goroutines 或程序调用( [source](https://golang.org/pkg/os/#Mkdir) )。此外，一旦您完成了对 temp 目录的操作，确保使用`[os.RemoveAll](https://golang.org/pkg/os/#RemoveAll)`删除它和它的内容。

## 导航和阅读目录

首先，我们可以用`[os.Getwd](https://golang.org/pkg/os/#Getwd)`获得当前的工作目录。

然后，可以使用`[os.Chdir](https://golang.org/pkg/os/#Chdir)`来改变工作目录。

除了改变工作目录，我们还可以用`[os.ReadDir](https://golang.org/pkg/os/#ReadDir)`得到目录子目录。`os.ReadDir`接受一个目录路径，如果不成功，返回一个`DirEntry`结构数组和一个非 nill 错误。

Source: [https://golang.org/pkg/io/fs/#DirEntry](https://golang.org/pkg/io/fs/#DirEntry)

下面是它的用法:

## 遍历目录

使用`os.Chdir` & `os.ReadDir`，我们可以遍历一个父目录中的所有文件和子目录，但是 [path/filepath](https://golang.org/pkg/path/filepath/) 包提供了一种使用`[filepath.WalkDir](https://golang.org/pkg/path/filepath/#WalkDir)`函数完成这一任务的优雅方式。

`filepath.WalkDir`接受`root`，将要行走的目录，以及一个如下类型的回调函数`fn`:

```
type WalkDirFunc func(path [string](https://golang.org/pkg/builtin/#string), d [DirEntry](https://golang.org/pkg/io/fs/#DirEntry), err [error](https://golang.org/pkg/builtin/#error)) [error](https://golang.org/pkg/builtin/#error)
```

`fn`将在`root`中的每个文件和子目录上被调用。下面是一个计算根目录下所有文件的例子。

`path/filepath`提供了另一个名为`[filepath.Walk](https://gist.github.com/Ramko9999/d49a3f944a6530c099367395f9dace68)`的函数，其行为与`filepath.WalkDir`相同。然而，文件表明`filepath.Walk`的效率低于`filepath.WalkDir`。因此，使用`filepath.WalkDir`可能更理想。

# 结论

我写这篇文章是作为学习 Go 的一个练习，我希望通过它向您介绍使用文件和目录的基础知识。作为练习，我建议实现您自己版本的`filepath.WalkDir`。

**感谢您的阅读！**