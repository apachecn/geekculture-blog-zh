# 如何在 Linux 上 GREP 内容？

> 原文：<https://medium.com/geekculture/how-to-grep-content-on-linux-3db0a0112618?source=collection_archive---------10----------------------->

![](img/1e9bf1382a326fd98f09fa1178a3fd54.png)

Photo by [Tyler Nix](https://unsplash.com/@nixcreative?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

选择性地过滤文本是大多数脚本包含的最重要的任务之一。然而，大多数初学 shell 脚本的人会以声明的方式来做，通过初始化一个变量，并使用一个带有条件的 for 循环，有选择地删除无用的文本。然而，有一种更简单的方法来执行相同的操作。只需使用名为 grep 的内置 GNU 命令。这个实用程序允许您接受一个用文本填充的输入，并输出通过匹配模式过滤的文本。

下面是 grep 命令在 Linux 上的工作方式。

## grep 命令的结构

```
grep [OPTIONS] PATTERN [FILE ....]
grep [OPTIONS] [ -e PATTERN | -f FILE] [FILE...]
```

[注册我的电子邮件列表，如果这对你有帮助的话](/subscribe/@drechang)

## 解释了所有 grep 选项

**您将在大多数时间使用的选项**

*   -v，—反转匹配:选择不匹配的行
*   -c，— count:打印匹配行的计数
*   -w，— word-regexp:选择包含构成完整单词的匹配项的行。
*   -x，— line-regexp:选择与整行完全匹配的内容。
*   -r，-R，recursive:递归读取每个目录下的所有文件；相当于三维递归
*   — include=PATTERN:仅在目录中递归搜索文件匹配模式。
*   — exclude=PATTERN:在目录中递归跳过文件匹配模式
*   -n，—行号:用输入文件中的行号作为每行输出的前缀。
*   -o，— only-matching:仅显示匹配行匹配模式的部分。
*   -G，— basic-regexp:将模式解释为基本正则表达式。默认选项
*   -H，--with-filename:打印每个匹配项的文件名。
*   -i，— ignore-case:忽略大小写区别
*   -E，— extended-regexp:将模式解释为扩展正则表达式
*   -e 模式，— regexp=PATTERN:使用模式作为模式；对保护有用
*   -F，—固定字符串:将模式解释为固定字符串的列表
*   -P，— perl-regexp:将模式解释为 perl 正则表达式。

**您偶尔会用到的选项**

*   -A NUM，— after-context=NUM:在匹配行之后打印行数。
*   -a，— text:相当于— binary-files=text 选项。
*   -B NUM，— before-context=NUM:匹配行之前的打印行数。
*   -C NUM，— context=NUM:打印行数
*   -b，— byte-offset:在每个输出行之前打印输入文件中的字节偏移量
*   —COLOR[= WHEN]，— color=[=WHEN]:用 GREP_COLOR env 变量中的 market 将匹配字符串括起来。WHEN 可以是“从不”、“总是”或“自动”。
*   —帮助:输出简短的帮助消息。
*   -m NUM，-max-count=NUM:在 NUM 个匹配行之后停止读取。
*   — mmap:对系统调用使用 mmap 而不是 read。可能会产生更好的性能。
*   — label=LABEL:显示实际来自标准输入的输入
*   —行缓冲:使用行缓冲
*   -q，—安静，—无声:安静；不要向标准输出中写入任何内容。相当于-s 或-no-messages 选项。
*   -s，— no-messages:将输出重定向到/dev/null
*   -U，— binary:将文件视为二进制文件，而不是文本
*   -u，— unix-byte-offsets:报告 unix 样式的字节偏移量。
*   -V，— version:打印 grep 的版本号
*   -y:和-i 一样
*   -Z，— null:输出零字节而不是字符

[注册我的电子邮件列表，如果这对你有帮助的话](/subscribe/@drechang)

## 使用 grep 的示例

**查找所有隐藏的目录和目录下的文件(非递归)**

命令

```
ls -al | grep ".*"
```

输出

```
total 308
drwx------ 39 andre andre  4096 Mar 18 14:01 .
drwxr-xr-x  4 root  root   4096 Jun 24  2021 ..
drwxr-x---  2 andre andre  4096 Nov 16  2020 .android
drwxr-xr-x  3 andre andre  4096 Nov 16  2020 .ApacheDirectoryStudio
drwxr-xr-x  2 andre andre  4096 Aug 10  2021 Applications
-rw-r--r--  1 andre andre    57 Feb 17  2020 .bash_profile
-rw-r--r--  1 andre andre  3838 Feb 17  2020 .bashrc
drwxr-xr-x 63 andre andre  4096 Feb 22 18:17 .cache
drwx------  2 andre andre  4096 Aug 10  2021 .cdw
drwxr-xr-x 76 andre andre  4096 Mar 18 14:01 .config
-rw-r--r--  1 andre andre  4855 Oct 29  2017 .dir_colors
drwxr-xr-x  8 andre andre  4096 Mar 18 13:40 Documents
drwxr-xr-x  8 andre andre  4096 Jul 14  2021 .dotfiles
drwxr-xr-x 11 andre andre 20480 Mar 18 14:01 Downloads
drwxr-xr-x  3 andre andre  4096 Nov 16  2020 .eclipse
-rwxr-xr--  1 andre andre    89 Mar 17 17:22 .fehbg
drwx------  3 andre andre  4096 Oct  8  2020 .fltk
-rw-r--r--  1 andre andre  1121 Jul 14  2021 .gitignore
drwx------  4 andre andre  4096 Feb 22 11:13 .gnupg
drwxr-xr-x  2 andre andre  4096 Dec 24 13:41 .gphoto
drwxr-xr-x  8 andre andre  4096 Jul  9  2021 .gradle
-rw-r--r--  1 andre andre    31 Dec 21  2020 .gtk-bookmarks
-rw-r--r--  1 andre andre   300 Jun 25  2021 install.sh
...
```

**获取给定目录中的文件和目录计数(非递归)**

命令

```
ls -al | grep "" -c
```

输出

```
63
```

[注册我的电子邮件列表，如果这对你有帮助的话](/subscribe/@drechang)

## 了解有关 grep 命令的更多信息

在终端中键入以下命令，了解有关 grep 命令的更多信息。

```
man grep
```

IT 和工程领域是快速发展的领域。跟不上意味着你将被落在后面。跟上的最好方法是保持最新的新闻和教育内容。[订阅免费电子邮件列表，将您的职业生涯提升 10 倍。](/subscribe/@dretechtips)

**加入我们，因为 50 多名想要快速跟踪其职业生涯和知识基础的人已经注册。**

**相关内容**

*   [尴尬的介绍](/geekculture/an-awkward-introduction-c672cc0490dd?source=your_stories_page----------------------------------------)
*   [为什么 Windows 比 Linux 好？](/@drechang/why-windows-is-better-than-linux-da410b8d9689?source=list-6214c3278ac0--------6-------8e08eeb9d883------------------------)
*   [如何在 Linux 中查找文件？](/geekculture/how-to-find-files-in-linux-6ed09a98c899)