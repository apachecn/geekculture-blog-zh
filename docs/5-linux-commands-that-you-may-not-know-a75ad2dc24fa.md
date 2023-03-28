# 你可能不知道的 5 个 Linux 命令

> 原文：<https://medium.com/geekculture/5-linux-commands-that-you-may-not-know-a75ad2dc24fa?source=collection_archive---------10----------------------->

当谈到 Linux 时，新手用户认为它是一个复杂的操作系统，因为每项工作都是通过终端使用命令来完成的。但是当你开始在 Linux 上工作时，你会喜欢这些命令，在 Linux 上工作一段时间后，你会开始喜欢 Linux 而不是 windows。此外，通过终端使用命令执行任务要比使用 Linux 的 GUI 快得多。

在这里，我将告诉你那些不经常使用的命令，但是如果你知道它们会更有用。这些是:

1.  **find:** find 用于查找目录中的任意文件。它搜索整个目录，并返回符合给定搜索条件的文件的名称。该命令的语法如下:

```
**Syntax: find directory_name -name file_name
Example:** ***find /home -name hello.txt,*** *will find out the hello.txt in the home directory and its subdirectories.* 
```

2. **sed:** 代表流编辑器。它可以执行许多任务，如搜索、查找和替换。对于在基于图形用户界面的编辑器中打开文件需要花费大量时间的较大文件来说，它就像一个魔术。它支持正则表达式，这允许它执行复杂的模式匹配。该命令的语法如下:

**2.1 用新文本替换旧文本:**该命令将用新文本替换给定文件中的旧文本。

```
**Syntax: sed ‘ -s/old_text/new_text’ fileName
Example:** **sed ‘ -s/Linux/Unix’ *hello.txt,*** *will replace  the word ‘Linux’ with ‘Unix’ in hello.txt in.*
```

**2.2** 如果一行只包含一个实例，那么它将不会被替换。

```
**Syntax: sed ‘ -s/old_text/new_text/n’ fileName
Example:sed ‘ -s/Linux/Unix/2’ *hello.txt,*** *will replace the 2nd occurrence of the word ‘Linux’ with ‘Unix’ in hello.txt in every line wherever 2 occurrences of the word ‘Linux’ are present.*
```

**2.3** **替换特定行号上的字符串:**您可以限制 sed 命令替换特定行号‘k’上的字符串。

```
**Syntax: sed ‘k -s/old_text/new_text/n’ fileName
Example: sed ‘3 -s/Linux/Unix’ *hello.txt*** *It will replace the word ‘Linux’ with ‘Unix’ in 3rd line of  hello.txt.*
```

**2.4**

```
**Syntax: sed ‘n,m -s/old_text/new_text/n’ fileName
Example: sed ‘3,7 -s/Linux/Unix’ *hello.txt*** *It will replace the word ‘Linux’ with ‘Unix’ from line number 'm' to line number 'n' in  hello.txt.*
```

**2.5** **从特定文件中删除行:**你也可以使用 sed 命令删除一行。与替换类似，可以在给定文件名**中指定要删除的特定行号或行号范围。**

```
***2.5.1 To delete a particular line:*
Syntax: sed ‘nd’ fileName 
Example: sed ‘3d’ *hello.txt*** *It will delete the 'nth' in  hello.txt.****2.5.2 To delete a range of particular lines:*
Syntax: sed ‘n,md’ fileName 
Example: sed ‘3,7d’ *hello.txt*** *It will delete from line number 3 to line number 7 in hello.txt.****2.5.3 To delete a pattern matching line:*
Syntax: sed ‘/pattern/d’ fileName 
Example: sed ‘/Linux/d’ *hello.txt*** *It will delete all those lines which contains 'Linux' word in  hello.txt.*
```

**3。watch:** watch 命令用于在特定的时间间隔后执行任何命令。例如，如果您希望每 10 秒执行一个命令，那么您可以在该命令前面键入 watch，这样您就可以每 10 秒检查一次该命令的输出。

```
***General Syntax:watch [option] command_to_execeute*****3.1 Syntax: *watch -d -n timeInSeconds command_to_execeute*
Example: *watch -d -n 10 free -m*** *It will excute 'free -m' command every 10 seconds, and it will hoghlights the differnces in the output of 'free -m' command from previous run to current run.* **3.2 Syntax: *watch -g -n timeInSeconds command_to_execeute*- :** 
**Example: *watch -g -n 10 free -m***This will run 'free -m' command every 10 seconds and it treminates when the output of command changes from previous run.
```

**4。df:** 这是你可以用来检查系统磁盘空间的命令。默认情况下，它以 KB 和百分比显示空间。

```
**4.1 Syntax: *df***
**Example: *df*** *This will show disk space in KBs and percenetage.***4.1 Syntax: *df -m***
**Example: *df -m*** *This will show the disk space in MBs and percenetage.*
```

**5。ifconfig:** 用来显示你所在网络的网络信息。它将显示您网络的所有 IP 地址、子网掩码和默认网关。

```
**5.1 Syntax: *ifconfig***
**Example: *ifconfig*** *This will show* network information of you sysetm network.
```

我希望您喜欢阅读这篇文章并获得关于这些命令的信息。我希望你能尽快使用这些命令。如果你喜欢这篇文章，那就在 medium 上关注我。谢了。