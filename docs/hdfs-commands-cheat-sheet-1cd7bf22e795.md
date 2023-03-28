# HDFS 命令备忘单

> 原文：<https://medium.com/geekculture/hdfs-commands-cheat-sheet-1cd7bf22e795?source=collection_archive---------1----------------------->

HDFS 命令快速指南

![](img/f7a8def6401da8e91da4ad8fce319df8.png)

HDFS 是 Hadoop 生态系统的主要枢纽，负责跨各种节点存储结构化和非结构化的大型数据集，从而以日志文件的形式维护元数据。因此，要使用这样一个系统，我们需要精通或者至少应该知道常用的命令和过程来简化我们的任务。在这个问题上，我们已经巩固了一些最常用的 HDFS 命令，你应该知道与 HDFS 合作。

首先，我们需要检查下面的列表。

1. [**安装 Hadoop**](/p/61b0e67342f8)

2.**运行 Hadoop** —我们可以使用‘start-all . cmd’命令或者直接从 Hadoop 目录启动。

3.**验证 Hadoop 服务** —我们可以使用下面的命令检查我们的 Hadoop 是否启动并运行。

**jps**

![](img/1c4551d2bfc5693a5ff5c6a815a9b044.png)

伟大的..！！！现在我们准备好执行和学习命令。

> * *注意:-这些命令是特定于大小写的。写命令时，一定要特别注意大写和小写字母。

1.  **version —** 该命令用于了解 Hadoop 的版本，以及附加的本地文件系统位置和编译信息。

## ***hadoop 版本***

![](img/9ab04351deedf0befb7cc13aa1c1cf8b.png)

hadoop version

**2。mkdir —** 该命令用于创建一个新目录，如果它不存在的话。如果目录存在，它将给出一个“文件已经存在”的错误。

## hadoop fs -mkdir

![](img/9b186f8204bced707aec2d0c1c9b94b3.png)

mkdir

**3。ls —** 该命令用于检查 HDFS 中的文件或目录。它显示指定目录中每个文件或目录的名称、权限、所有者、大小和修改日期。

## hadoop fs -ls

![](img/c97aa65f89bf99cd0761e2bc86ad4262.png)

hadoop ls

**4。put —** 该命令用于将数据从本地文件系统复制到 HDFS。

## Hadoop fs-put<local file="" path=""></local>

![](img/5eb18325f7644272e52d38c8d510318b.png)

hadoop put

我们可以从 HDFS 网站上验证这一点。

![](img/09719a9ad6c84cacb0222d9644a1a5f2.png)

hadoop put webUI

**5。get —** 该命令用于将数据从 HDFS 复制到本地文件系统。该命令与“上传”命令相反。

## hadoop fs -get<hdfs file="" path=""><local file="" path=""></local></hdfs>

![](img/44f4b367e26e36302210481856f076de.png)

hadoop get

我们可以从本地文件系统中验证这一点。

![](img/10dbed629fed8f154763c1e048a0f172.png)

hadoop get webUI

6。cat — 命令用于查看 HDFS 文件中的数据

## hadoop fs -cat

![](img/6423573f190fb16a527c8d4af85c2496.png)

hadoop cat

**7。mv —** 该命令用于将文件从 HDFS 的一个位置移动到 HDFS 的另一个位置。

## hadoop fs -mv <destination hdfs="" path=""></destination>

![](img/2e30483f61f96d2f5b3e0186530c0661.png)

hadoop mv

我们可以从 Web 用户界面验证这一点。

![](img/95b318aea2071ad75420e0f715eac857.png)

hadoop mv webUI

**8。cp —** 该命令用于将文件从 HDFS 的一个位置复制到 HDFS 境内的另一个位置。

## hadoop fs -cp <destination hdfs="" path=""></destination>

![](img/dec97a58b82bfdf54397f5897164e992.png)

hadoop cp

我们可以从 Web 用户界面验证这一点。

![](img/d8e4a44a7057fc23cc129234b5cb8dd9.png)

hadoop cp webUI

**9。copyFromLocal —** 该命令用于将数据从本地文件系统复制到 HDFS。

## Hadoop fs-copy from local<local file="" path=""></local>

![](img/a8c31483b7800baa5e3b9495f7ad15aa.png)

Hadoop copyFromLocal

我们可以从 WebUI 验证复制的文件。

**10。copyToLocal —** 该命令用于将数据从 HDFS 复制到本地文件系统。

## hadoop fs -copyToLocal<hdfs file="" path=""><local file="" path=""></local></hdfs>

![](img/91175863704e9ed9680ecccd25faeb2f.png)

hadoop copyToLocal

我们可以在本地文件系统中检查该文件。

**11。movefromlocial—**该命令用于将文件或目录从本地文件系统移动到 HDFS。

## Hadoop fs-movefromlocial<local file="" path=""></local>

![](img/cea82d9d0eb1b2470ce474e0070943f4.png)

hadoop moveFromLocal

**12。moveToLocal —** 该命令用于将文件或目录从 HDFS 移动到本地文件系统。该命令尚未实现，但很快就会实现。

## Hadoop fs-moveToLocal<hdfs file="" path=""></hdfs>

![](img/3934e917bcd15beab3fe072aa374d4fa.png)

hadoop moveToLocal

**13。rm —** 删除，该命令用于从 HDFS 中删除/移除文件。

## hadoop fs -rm

![](img/cb885f3dfedf17e1e2afe9d1afd587f3.png)

hadoop rm

**14。tail —** 该命令用于从 HDFS 读取文件的尾部/结尾部分。它有一个额外的参数“[-f]”，用于显示文件的附加数据。

## hadoop fs -tail [-f]

![](img/75108c7a9a278ec0026e48a48c82b58a.png)

hadoop tail

15。expunge — 该命令用于清空垃圾箱。

## hadoop fs -expunge

![](img/5d2c523bdc3e001c1322821458fe695c.png)

hadoop expunge

16。chown — 当我们想在 HDFS 改变一个文件或目录的**用户**时，我们应该使用这个命令。

## hadoop fs -chown

![](img/38d255bbbd016c951f45c4b8e72584b6.png)

hadoop chown

我们可以使用 hadoop -ls 命令或从 WebUI 验证用户是否进行了更改。

![](img/3c28d13f85a66a0c08cdc03a00272e39.png)

hadoop chown WebUI

17。chgrp — 当我们想在 HDFS 改变一个文件或目录的**组**时，我们应该使用这个命令。

## hadoop fs -chgrp

![](img/ab89aa00eb3b070ebe808d09f8f88f15.png)

hadoop chgrp

我们可以使用 hadoop -ls 命令或从 WebUI 验证用户是否进行了更改。

![](img/e333a1200aec94e1141c86f2bf595c26.png)

hadoop chgrp WebUI

18。setrep — 该命令用于在 HDFS 中更改文件的复制因子。

## hadoop fs -setrep<replication factor=""></replication>

![](img/f6a8500460e3d044aa5338e857f3df26.png)

hadoop setrep

我们可以从网络界面查看。

![](img/e2fa8aeca2ad413fdecfac74d9812a15.png)

hadoop setrep WebUI

19。du — 该命令用于检查文件或目录的磁盘使用量。

## hadoop fs -du

![](img/9ac5cbbd7ae9dee99fdbcf23e9418af3.png)

hadoop du

**20。df —** 此命令用于显示 HDFS 文件系统的容量、可用空间和大小。它有一个额外的参数“[-h]”，用于将数据转换为人类可读的格式。

## hadoop fs -df [-h]

![](img/6fbe9ef2e0ce1fb6fd00bda7adea6da1.png)

hadoop df

**21。fsck —** 此命令用于检查 HDFS 文件系统中文件的健康状况。

## hadoop fsck

![](img/c6d0a846207162d201c5d7f77bb899b8.png)

hadoop fsck

它还有一些属性/选项来修改命令的用法。

![](img/1ed3154768afcb2c77d912c9c8a14087.png)

hadoop fsck options

**22。touchz —** 该命令在指定的目录中创建一个大小为 0 的新文件。

## hadoop fs -touchz

![](img/b80c71e4d77b0f43209b6e488f09ed5e.png)

hadoop touchz

新文件可以在 WebUI 中看到。

![](img/ff261e04cbba7a200f616d3bf4946bc7.png)

hadoop touchz webUI

**23。test —** 该命令回答关于< HDFS 路径>的各种问题，并通过退出状态给出结果。

## hadoop fs -test

![](img/8351da1dc71278455ef00f6837ce9587.png)

hadoop test options

**24。text —** 这是一个简单的命令，用于在控制台上打印 HDFS 文件的数据。

## hadoop fs -text

![](img/c57320dee60a2db045568922408911d6.png)

25。stat — 该命令提供 HDFS 中文件或目录的 stat。

## hadoop fs -stat

![](img/e4de80da435c2c557a31fccb12c58033.png)

hadoop stat

它可以提供以下格式的数据。默认情况下，它使用“%y”。

![](img/cabb5cfbe422dfbe40eb4eab8552d68b.png)

hadoop stat formats

**26。用法—** 显示给定命令或所有命令(如果未指定)的用法。

## Hadoop fs-用法<command></command>

![](img/f4036522a901e6cda8ea60b0f412c059.png)

hadoop usage

**27。帮助—** 显示给定命令或所有命令(如果未指定)的帮助。

## hadoop fs -help<command></command>

![](img/f915c8cd294d96046f50a51dbf7d164e.png)

hadoop help

**28。chmod —** 用于更改 HDFS 文件系统中文件的权限。

## hadoop fs -chmod [-r]

![](img/26c3ef2230762c27199386deff4c8c4d.png)

hadoop chmod

**旧权限**

![](img/65ae8b131967f441f11e08917a344573.png)

hadoop chmod old permission

**新权限**

![](img/0c072aa143dfee7cfad6e0d23132065b.png)

hadoop chmod new permission

29。appendToFile — 该命令用于将本地文件系统中的两个文件合并为 HDFS 文件中的一个文件。

## Hadoop fs-appendToFile<local file="" path1=""><local file="" path2=""></local></local>

![](img/417bd05d353ebee51625a0be14e485a0.png)

hadoop appendtofile

![](img/1be3b524390d35a0767575897d20bf2e.png)

hadoop appendtofile webui

三十岁。校验和— 该命令用于检查 HDFS 文件系统中文件的校验和。

## hadoop 文件系统校验和

![](img/bf8f3668d3e092eb96d8da34d8310675.png)

hadoop checksum

**三十一。count —** 计算特定路径下文件、目录的数量和大小。

## Hadoop fs-count[选项]

![](img/3a9a39766bd8c1b0efa9c5c2445bd154.png)

hadoop count

这个函数也有一些函数可以根据需要修改查询。

![](img/44dc1860b1aeba31794137de320c512a.png)

hadoop count options

**32。查找—** 该命令用于在 HDFS 文件系统中查找文件。我们需要提供我们正在寻找的表达式，并且如果我们想要在特定的目录中寻找文件，也可以提供一个路径。

## hadoop fs -find

![](img/069c9fe1edcc505d18c8cbf4e9bdace0.png)

hadoop find

**33。getmerge —** 该命令用于将 HDFS 目录的内容合并到本地文件系统的一个文件中。

## Hadoop fs-get merge<hdfs directory=""></hdfs>

![](img/0e95588c498d70b2f4928e78a59a4d05.png)

hadoop getmerge

可以在本地文件系统中看到合并的文件。

![](img/66af9014eaeb9814dc932b55a53d7cba.png)

hadoop getmerge file

HDFS 命令概述。

![](img/c629399638d181463fb2d8b362e6ffbc.png)

HDFS command Cheat codes

# 摘要

我们学习了最常见和最常用的 HDFS 命令。我们已经看到了如何使用它们，也学到了实用的方面。

所以不要只是阅读，和我们一起实践，让我们知道你面临的问题和挑战。

在那之前…这是快速数据科学团队签署。另一篇文章再见。