# 在 Linux 环境中管理文件

> 原文：<https://medium.com/geekculture/managing-files-in-a-linux-environment-50d4d6ad6d49?source=collection_archive---------26----------------------->

![](img/19f7f1aa295a46d4f8481b023bae1a9a.png)

本文为您提供了在 Linux 环境中管理文件的基础。

# 介绍

Linux 系统是由基于其根的树形结构组织的。我们以这种方式访问所有文件和目录。

# 先决条件

您将需要一个工作的 Linux 系统来跟进。我建议这样做，因为它有助于您学习 Linux 文件系统并对其更加熟悉。

# 文件路径

文件名是绝对的或相对的。绝对路径以/开头。我们用/分隔路径中的每个目录。你总是可以用几种不同的方式来判断你在哪里。我们通过使用‘$ pwd’变量并在您的终端中使用‘pwd’命令来实现这一点。

# 列出文件详细信息

关于文件的信息记录在一个 inode 中。当您查询 inode 时，它会为您提供大量信息。其中包括所有者、创建日期、大小和访问权限。无论如何，当你使用带有' l '选项的' ls '命令时，它会给你一些提示。

```
ls -l
total 231
-rw-r--r-- 1 Jason 197121     9 Jul 14 18:04  README.md
-rw-r--r-- 1 Jason 197121    64 Jul 14 18:04 'compiling and running'
-rw-r--r-- 1 Jason 197121   160 Jul 14 22:15  variables1.cpp
-rwxr-xr-x 1 Jason 197121 44458 Jul 14 22:15  variables1.exe*
-rw-r--r-- 1 Jason 197121   170 Jul 14 22:21  variables2.cpp
-rwxr-xr-x 1 Jason 197121 44599 Jul 14 22:21  variables2.exe*
-rw-r--r-- 1 Jason 197121   415 Jul 14 22:38  variables3.cpp
-rwxr-xr-x 1 Jason 197121 45334 Jul 14 22:38  variables3.exe*
-rw-r--r-- 1 Jason 197121   175 Jul 14 22:43  variables4.cpp
-rwxr-xr-x 1 Jason 197121 44511 Jul 14 22:43  variables4.exe*
-rw-r--r-- 1 Jason 197121   373 Jul 14 22:54  variables5.cpp
-rwxr-xr-x 1 Jason 197121 44934 Jul 14 22:54  variables5.exe*
```

您可以看到'-l '选项为您提供了更多有用的信息。它也以更好的方式呈现。

除非您要求,' ls '命令也列出隐藏文件，否则它不会列出。您可以将' a '选项与' ls '一起使用来查看隐藏的文件和目录。要获得文件的最佳视图，请在终端中使用“ls -al”。这将向您显示所有文件，并为您提供每个文件的大量信息。

```
ls -al
total 243
drwxr-xr-x 1 Jason 197121 0 Jul 14 22:54 ./
drwxr-xr-x 1 Jason 197121 0 Jul 14 18:04 ../
drwxr-xr-x 1 Jason 197121 0 Jul 14 18:04 .git/
-rw-r--r-- 1 Jason 197121 9 Jul 14 18:04 README.md
-rw-r--r-- 1 Jason 197121 64 Jul 14 18:04 'compiling and running'
-rw-r--r-- 1 Jason 197121 160 Jul 14 22:15 variables1.cpp
-rwxr-xr-x 1 Jason 197121 44458 Jul 14 22:15 variables1.exe*
-rw-r--r-- 1 Jason 197121 170 Jul 14 22:21 variables2.cpp
-rwxr-xr-x 1 Jason 197121 44599 Jul 14 22:21 variables2.exe*
-rw-r--r-- 1 Jason 197121 415 Jul 14 22:38 variables3.cpp
-rwxr-xr-x 1 Jason 197121 45334 Jul 14 22:38 variables3.exe*
-rw-r--r-- 1 Jason 197121 175 Jul 14 22:43 variables4.cpp
-rwxr-xr-x 1 Jason 197121 44511 Jul 14 22:43 variables4.exe*
-rw-r--r-- 1 Jason 197121 373 Jul 14 22:54 variables5.cpp
-rwxr-xr-x 1 Jason 197121 44934 Jul 14 22:54 variables5.exe*
```

你可以在顶部看到隐藏的目录。Git 正在跟踪这个目录，所以我们看到我们通常的。git '文件。

在查看目录结构时，另一个很好的选项是'-h '选项。这是人类可读的格式，而不是机器。没有太大的区别，但它使文件大小更容易阅读。

```
ls -lh
total 231K
-rw-r--r-- 1 Jason 197121   9 Jul 14 18:04  README.md
-rw-r--r-- 1 Jason 197121  64 Jul 14 18:04 'compiling and running'
-rw-r--r-- 1 Jason 197121 160 Jul 14 22:15  variables1.cpp
-rwxr-xr-x 1 Jason 197121 44K Jul 14 22:15  variables1.exe*
-rw-r--r-- 1 Jason 197121 170 Jul 14 22:21  variables2.cpp
-rwxr-xr-x 1 Jason 197121 44K Jul 14 22:21  variables2.exe*
-rw-r--r-- 1 Jason 197121 415 Jul 14 22:38  variables3.cpp
-rwxr-xr-x 1 Jason 197121 45K Jul 14 22:38  variables3.exe*
-rw-r--r-- 1 Jason 197121 175 Jul 14 22:43  variables4.cpp
-rwxr-xr-x 1 Jason 197121 44K Jul 14 22:43  variables4.exe*
-rw-r--r-- 1 Jason 197121 373 Jul 14 22:54  variables5.cpp
-rwxr-xr-x 1 Jason 197121 44K Jul 14 22:54  variables5.exe*
```

您会注意到，在大多数清单中，一些基本信息被反复显示。这就是“ls”命令的特点。

该清单向您展示了这些属性:

*   文件或目录
*   文件的链接
*   文件的所有者
*   所有者团体
*   文件大小(以字节为单位)
*   印时戳
*   上次修改
*   文件或目录的名称

另一个有用的选项是“I”和“ls”。这将为您提供一个文件的 inode 信息。

```
ls -li
total 231
104427216359722182 -rw-r--r-- 1 Jason 197121     9 Jul 14 18:04  README.md
 24206847997189317 -rw-r--r-- 1 Jason 197121    64 Jul 14 18:04 'compiling and running'
  9851624184970397 -rw-r--r-- 1 Jason 197121   160 Jul 14 22:15  variables1.cpp
  3659174697337710 -rwxr-xr-x 1 Jason 197121 44458 Jul 14 22:15  variables1.exe*
 17169973579462677 -rw-r--r-- 1 Jason 197121   170 Jul 14 22:21  variables2.cpp
  8444249301432886 -rwxr-xr-x 1 Jason 197121 44599 Jul 14 22:21  variables2.exe*
 56576470318862464 -rw-r--r-- 1 Jason 197121   415 Jul 14 22:38  variables3.cpp
  2533274790509358 -rwxr-xr-x 1 Jason 197121 45334 Jul 14 22:38  variables3.exe*
  5348024557584218 -rw-r--r-- 1 Jason 197121   175 Jul 14 22:43  variables4.cpp
  7036874417879868 -rwxr-xr-x 1 Jason 197121 44511 Jul 14 22:43  variables4.exe*
 27866022694408530 -rw-r--r-- 1 Jason 197121   373 Jul 14 22:54  variables5.cpp
 13510798882225059 -rwxr-xr-x 1 Jason 197121 44934 Jul 14 22:54  variables5.exe*
```

inode 信息在最左边的开头。这可能非常有用。

您可以使用' ls -R '列出目录及其子目录。这只列出了目录，而不是其中的文件。

我要展示的最后一个“ls”的例子是另一个我非常喜欢的例子。它是“-S”选项，代表大小。这将按大小顺序列出文件，我在组织时经常使用这个。

```
ls -lS
total 231
-rwxr-xr-x 1 Jason 197121 45334 Jul 14 22:38  variables3.exe*
-rwxr-xr-x 1 Jason 197121 44934 Jul 14 22:54  variables5.exe*
-rwxr-xr-x 1 Jason 197121 44599 Jul 14 22:21  variables2.exe*
-rwxr-xr-x 1 Jason 197121 44511 Jul 14 22:43  variables4.exe*
-rwxr-xr-x 1 Jason 197121 44458 Jul 14 22:15  variables1.exe*
-rw-r--r-- 1 Jason 197121   415 Jul 14 22:38  variables3.cpp
-rw-r--r-- 1 Jason 197121   373 Jul 14 22:54  variables5.cpp
-rw-r--r-- 1 Jason 197121   175 Jul 14 22:43  variables4.cpp
-rw-r--r-- 1 Jason 197121   170 Jul 14 22:21  variables2.cpp
-rw-r--r-- 1 Jason 197121   160 Jul 14 22:15  variables1.cpp
-rw-r--r-- 1 Jason 197121    64 Jul 14 18:04 'compiling and running'
-rw-r--r-- 1 Jason 197121     9 Jul 14 18:04  README.md
```

# 分类输出

在“ls”如何向您提供输出方面，您有一些选项。按字母顺序是默认输出。然而，这很容易改变。按时间修改，用‘ls-t’，挺有用的。您可以添加“r”选项来反转时间列表，这给了您很大的灵活性。

# 复制文件

复制文件是一项经常发生的任务。为此，我们使用“cp”命令。它可以复制文件或目录。一次复制多个文件或目录也是一项简单的任务。要使用“cp ”,您必须给出源和目标名称。源必须包含路径。

```
$ cp file1 dir1
```

如果你的目标是一个目录，那么你的源代码将被放入其中。

递归复制也是一项简单的任务。你用“cp -R”来做这个。但是，不能将源目录作为目标。

# 移动文件

我用“mv”命令来做这件事。它用于移动或重命名文件和目录。它主要遵循与“cp”命令相同的规则。这种行为与复制一个文件，然后删除原始文件是一样的。

```
mv file1 dir2
```

这个简单的例子只是将“文件 1”移动到目录“目录 2”中。这些都在同一个文件夹里。但是，如果不在同一个目录中，只需添加文件和目录的路径。

# 删除文件

您可以使用“rm”命令删除文件。它可以一次删除多个文件或目录。你应该一直小心做这件事，并做好备份，以防万一。基本用法是这样的。

```
rm filename
```

互联网上流传着许多使用“rm”删除系统文件并杀死系统的人的故事。发生这种情况是因为他们没有真正理解他们删除了什么。

```
$ ls -l
total 0
-rw-r--r-- 1 Jason 197121 0 Jul 16 08:13 file1$ rm file1$ ls -l
total 0
```

这很好也很简单，但是如果需要的话，可以添加路径。删除目录是这样完成的:

```
$ rm -R dir1
```

默认情况下，目录只有在为空时才能被删除。因此，如果您需要清除某些东西，如何做的信息在“手册”页面中。我会让你在那里找它。大多数人不应该需要这样做。

“rm -i”命令和选项是一个很好的保护措施，您应该使用它。您可以为“rm”设置一个等同于“rm -i”的别名，这将是一个额外的预防措施。

# 制作目录

您可以通过“mkdir”命令创建目录。它可以是像“mkdir 音乐”这样简单的东西。使用“mkdir 音乐视频”一次创建几个目录。您可以通过使用“ls”命令来查看新创建的目录，以确保其正确运行。

```
mkdir music[[email protected]](https://aindien.com/cdn-cgi/l/email-protection) MINGW64 ~/Documents/atom/cpp (main)
$ ls -l
total 231
-rw-r--r-- 1 Jason 197121 9 Jul 14 18:04 README.md
-rw-r--r-- 1 Jason 197121 64 Jul 14 18:04 'compiling and running'
drwxr-xr-x 1 Jason 197121 0 Jul 16 08:22 dir1/
drwxr-xr-x 1 Jason 197121 0 Jul 16 08:17 dir2/
drwxr-xr-x 1 Jason 197121 0 Jul 16 08:28 music/
-rw-r--r-- 1 Jason 197121 160 Jul 14 22:15 variables1.cpp
-rwxr-xr-x 1 Jason 197121 44458 Jul 14 22:15 variables1.exe*
-rw-r--r-- 1 Jason 197121 170 Jul 14 22:21 variables2.cpp
-rwxr-xr-x 1 Jason 197121 44599 Jul 14 22:21 variables2.exe*
-rw-r--r-- 1 Jason 197121 415 Jul 14 22:38 variables3.cpp
-rwxr-xr-x 1 Jason 197121 45334 Jul 14 22:38 variables3.exe*
-rw-r--r-- 1 Jason 197121 175 Jul 14 22:43 variables4.cpp
-rwxr-xr-x 1 Jason 197121 44511 Jul 14 22:43 variables4.exe*
-rw-r--r-- 1 Jason 197121 373 Jul 14 22:54 variables5.cpp
-rwxr-xr-x 1 Jason 197121 44934 Jul 14 22:54 variables5.exe*
```

使用“mkdir -p /music/albums/BigBang”创建嵌套目录。这将创建 BigBang 目录以及两个父目录。

```
$ mkdir -p music/albums/BigBang
```

# 删除目录

使用“rm”命令可以轻松删除目录。它还有'-p '选项。必须谨慎使用“rm”命令。删除目录及其文件可能是危险的，所以请备份。默认情况下，目录必须为空，然后才能删除它们。

```
rmdir dir1
```

删除目录也可以递归完成。我通过使用“rmdir -R”命令来实现这一点。同样，请小心这样做。你不应该经常这样做，如果有的话。我将犹豫地添加“-f”选项来强制通过。如果系统需要清理，并且我没有所有的文件，我会这样做。

# 触摸文件

触摸命令是一个多用途命令。它可以做几件事，如更新文件访问时间，修改时间，创建空文件，并指定时间戳。在第一个例子中，我创建了一个空文件。

```
$ touch SuperJunior
```

使用“触摸”更新文件的修改时间。我通过使用“touch”将文件名作为参数来实现这一点。这会将时间戳设置为当前时间。

```
$ touch -a SuperJunior
```

如果您用作参数的文件不存在，则“触摸”会为您创建该文件。如果你不想让它为你创建文件，你可以给它一个'-c '选项。这将告诉它不要为你创建一个空文件。它看起来像“触摸 c 文件 1”。

```
$ touch -c SuperJunior
```

您可以使用“触摸”将文件的修改时间设置为您想要的任何值。它使用“YYMMddhhmmss”格式。所以，设置的时候可以非常精确。该命令看起来像“touch -t YYMMddhhmmss filename”。

# 使用查找

你用“find”来精确搜索。您可以指定几个标准。这些信息包括名称、时间戳、所有者、创建日期和大小。选择权在你。当您使用名称进行搜索时，它可以是名称的全部或一部分。您还可以在搜索模式中使用通配符。

```
$ find . -name variables1.cpp
./cpp/variables1.cpp
```

Find 会显示你要找的文件的路径。您也可以查找目录。

```
$ find / -type d -name cpp
```

搜索大小很容易，有几个选项。找到一个特定大小的文件是可行的。您还可以设置下限和上限来查找该范围内的任何内容。使用“size”时，“-c”参数代表字节，“-k”参数代表千字节。然后，你也可以像这样搜索空文件。-0 号。

# 获取文件详细信息

要获得文件的详细信息，您可以使用“文件”命令。由于 Linux 文件没有识别它们是什么的扩展名，这对于你了解它们是有用的。这是因为你需要知道用什么程序打开文件。

```
$ file music
music: directory
```

“文件”命令执行一些测试来完成这个任务。基本上，它检查文件是否为空，以及里面有什么数据。

现在，让我们看看一个文件，看看它是怎么回事。

```
$ file variables1.cpp
variables1.cpp: C source, ASCII text, with CRLF line terminators
```

这告诉了我们很多信息。毫无疑问，它应该告诉我们那是什么样的文件。我给了这个文件一个扩展名，但是如果它没有扩展名，那么这个信息对于知道它是什么类型的文件是至关重要的。我很健忘，所以这对我很有帮助。

# 压缩文件

压缩和解压缩文件是任何操作系统的必备技能。当你想备份或发送文件到某个地方时，压缩文件是很有用的。压缩和解压缩文件的最佳工具之一是“gzip”。

文本文件是最容易压缩的文件，也是最有效的文件。但是，如果二进制文件和图像文件成功，它们不会有相同的效果。使用“gzip filename”命令压缩文件。然后，您可以使用“gunzip 文件名”来解压缩它。

# 文件存档

文件、目录和文件系统都需要归档。有两个主要命令可以完成这项工作。它们是“tar”和“dd”命令。

归档工具的主要目的之一是执行备份，这也是我想讨论的内容。有三种主要的备份类型:增量备份、差异备份和完整备份。

增量备份是自上次增量备份以来的更改。从灾难中恢复需要最后一次完全备份和所有后续增量备份。

差异备份是自上次完全备份以来的所有更改。这些内容的恢复需要最后一次完整备份和最新的差异备份。

完整备份就是一切。它通常是一个完整的文件系统或所有的用户文件。恢复时间最长，因为它将拥有最多的数据。

# Tar 命令

“Tar”代表磁带存档。它将从源文件或目录列表中创建一个归档文件。恢复很容易，tar 命令也可以做到这一点。因此，您可以用一个命令处理所有归档过程。归档时的另一个好处是，当原始源是一个目录时，子目录会自动包含在内。

输出可以是文件、硬件或“标准输出”。这使得它非常灵活，你可以用任何你需要的方式来使用它。你用“-f”选择输出选项。要提取归档文件，可以使用'-x '选项。让我们从一个简单的例子开始。

```
$ tar -cfv cpp.tar /home/cpp/
```

还有几个选项我没有提到。“-c”选项意味着创建一个新的存档，而“-v”选项将向我们显示进度。制作的档案将被称为“cpp.tar”。它是“cpp”目录的存档。

我们可以创建一个存档文件，同时压缩它。这是一种流行的做法。我们将在“tar”命令中添加“-z”选项。

```
$ tar -cvfz cpp.tar.gz /home/cpp/
```

'-z '选项将用我们前面提到的' gzip '压缩' tar '文件。

当我们想从一个“tar”文件中提取文件时，我们使用“-x”选项。该命令现在将如下所示。

```
$ tar -vfx cpp.tar
```

这将把存档文件提取到当前目录。

当您想要解压缩一个归档文件时，同样的过程也是有效的。同样，您使用'-x '选项。

```
$ tar -vfx cpp.tar.gz
```

由于使用了'-x '选项，这将提取文件并将其解压缩。

你可以列出一个“tar”文件的内容，以防你忘记。您可以使用'-t '选项来完成这项工作。该命令现在看起来像这样。

```
$ tar -vft cpp.tar
```

它的工作方式与压缩存档完全相同。

```
$ tar -vft cpp.tar.gz
```

这向我们展示了我们最初归档和压缩了哪些文件。

# DD 命令

“dd”命令是另一种类型的复制命令。然而，它能做的远不止“cp”实用程序。它相当强大。“dd”命令可以对文件进行一些基本的转换，并写入原始设备。

基本用法是“dd if=source of=target”。如果你需要，它会使用“标准输入”和“标准输出”，但是它真正的能力是选择它做什么。源目标或目标目标都可以是裸设备。这使您可以轻松地使用磁盘。

你可以用“gzip”压缩你的存档文件，使用管道来减小文件的大小。我们可以在同一个命令中完成。如果您同时进行压缩，归档将需要更长的时间。所以，要意识到这一点。

让我们做一个基本的例子来看看它是如何工作的。

```
$ dd if=/dev/sdb of=/dev/sdc
```

这只是将一个设备直接复制到另一个设备。输出设备必须大于输入设备。

下一个选项是使用“dd”命令创建映像。这在备份时非常方便。您可以通过设备或文件来实现这一点。

```
$ dd if=/dev/sdb of=/backups/sdb.img
```

这将创建“sdb”设备的备份“img”文件。备份不能太多。

我们还可以在备份时进行压缩。这是通过管道连接到“gzip”实用程序来完成的。

```
$ dd if=/dev/sdb | gzip -c >/backups/sdb.img.gz
```

正如你所看到的，这使得一个“img”文件也被压缩。该命令将输出通过管道传输到“gzip ”,后者会压缩“img”文件。

现在，我们需要检查恢复文件。你只需颠倒这个过程。

```
$ dd if=/backups/sdb.img of=/dev/sdb
```

这是到原始设备的直接拷贝。

我们可以执行相同的过程来恢复压缩的“img”文件。看起来是这样的。

```
$ gzip -dc /backups/sdb.img.gz | dd of=/dev/sdb
```

此命令使用“gzip”实用程序来解压缩“img”文件，并通过管道将其传输到“dd”命令。然后,“dd”命令将解压缩后的文件恢复到原始设备。实际上，你可以用“dd”命令做更多的事情，这绝对是我最喜欢的 Linux 命令之一。

# 结论

这是一个有趣的工作指南。在这篇文章中，我们讨论了列出目录内容、复制文件、移动文件、查找文件和目录、压缩文件和归档文件。我还遗漏了很多内容，但是如果你感兴趣的话，这些信息可以在‘man’页面上找到。

# 感谢阅读

如果你读到这里，我真的很感激。我希望我的向导能帮助别人，我也喜欢传播 Linux 知识。

如果你想加入我的时事通讯，你可以点击这里加入。

如果你不知道下一步该读什么，请尝试以下选项:

[*如何入门使用 Linux*](/geekculture/how-to-get-started-in-linux-7d09475eb037)

[*c++*](https://jason-46957.medium.com/learning-c-for-beginners-62ad50876d64)

[*在 C++中使用变量*](https://aindien.com/using-variables-in-c.html)

*原载于 2021 年 7 月 15 日 https://aindien.com*[](https://aindien.com/managing-files-in-a-linux-environment.html)**。**