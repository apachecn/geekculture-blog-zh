# 尴尬的介绍

> 原文：<https://medium.com/geekculture/an-awkward-introduction-c672cc0490dd?source=collection_archive---------13----------------------->

## 了解如何使用 awk 命令来解析文本

![](img/b9380eff130c0bc4726801f40f42cb81.png)

Photo by [Bernard Hermant](https://unsplash.com/@bernardhermant?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

从文件或命令输出中提取信息是大多数用户都会面临的任务。然而，执行手动解析是不方便的，尤其是当您需要在批处理中重复执行时。有一个 GNU util 命令叫做 awk。命令允许您自动操作文本文件。然而，初学者可能很难理解 awk 命令是如何工作的。

下面介绍一下笨拙的指挥作品。

## awk 命令能做什么？

![](img/a43551d4172772426977002c8102faf6.png)

Photo by [Yancy Min](https://unsplash.com/@yancymin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

该命令可以执行的操作:

*   逐行扫描文件
*   将每个输入行拆分成字段
*   将输入行/场与模式进行比较
*   对匹配的行执行操作

使用案例:

*   转换数据文件
*   生成格式化报告

在这里注册我的电子邮件列表。

## awk 的不同实现

awk 命令有多种实现方式。你有 nawk，gawk，甚至 mawk。Linux 中通常使用两种不同类型的 awk 命令。awk 和 GNU awk 在语法和功能上非常相似。GNU awk 是标准 awk 的一个分支。这意味着 awk 上的所有命令都应该在 gawk 上运行，然而，反过来却不是这样。这同样适用于 awk 的每一个分支。

[在这里注册我的电子邮件列表。](/subscribe/@drechang)

## awk 命令的结构

```
awk [options] 'selection _criteria { action }' [input_file] > [output_file]
```

[在这里注册我的电子邮件列表。](/subscribe/@drechang)

## 内置环境变量

awk 命令有内置的变量，比如$1、$2、$3 来表示您正在选择的列($0 表示整行)。

*   **NR** :输入记录或行数
*   **NF** :字段或列的数量
*   **FS** :字段分隔符。默认情况下，它是空白。
*   **RS** :记录分隔符。默认情况下，它是换行符或“\n”
*   **OFS** :输出场分隔符。默认情况下，它是空白。
*   **ORS** :输出记录分隔符。默认情况下，它是换行符或“\n”

在这里注册我的电子邮件列表。

## 使用 AWK 命令的简单示例

**从账户信息文件中提取账户名和姓**

大多数动态网站或网络应用只是美化了的 excel 电子表格。假设您想要提取一个帐户信息文件的名字和姓氏。

输入文件:

```
# accounts.csv
Nadine,Rudkin,nrudkin0@wikia.com,Male,200.61.156.1972,
Cesaro,Spearman,cspearman1@blinklist.com,Female,113.65.62.713,
Bryant,Wheway,bwheway2@infoseek.co.jp,Male,12.232.111.1934,
Starlene,Tomaello,stomaello3@google.com.au,Female,110.137.45.1035,
Abagael,Hawton,ahawton4@nbcnews.com,Female,199.194.154.1186,
Stacy,Lennard,slennard5@goo.ne.jp,Male,16.221.204.647,
Francis,Iacobetto,fiacobetto6@paypal.com,Female,123.92.45.988,
Ailina,Burdon,aburdon7@163.com,Female,207.240.235.2549,
Joline,Dymick,jdymick8@bluehost.com,Female,78.207.170.5710,
Irving,Odhams,iodhams9@wisc.edu,Female,80.81.222.23011,
Gisele,Pluthero,gplutheroa@apache.org,Male,39.211.29.15612,
Woody,Housden,whousdenb@epa.gov,Male,204.249.180.14313,
Taite,Dennistoun,tdennistounc@list-manage.com,Male,195.157.252.17814,
Terza,Mowday,tmowdayd@marketwatch.com,Female,91.8.171.23115,
Vida,Croome,vcroomee@paypal.com,Male,119.6.60.16016,
Perle,Renoden,prenodenf@marriott.com,Female,198.239.236.9417,
Rawley,Grayham,rgrayhamg@samsung.com,Female,103.197.116.4618,
Kippar,Deeming,kdeemingh@tumblr.com,Male,60.73.105.18819,
Margery,Devereu,mdevereui@cnet.com,Female,44.159.148.23220,
Fenelia,Ivanitsa,fivanitsaj@uiuc.edu,Male,48.58.78.174
```

下面是命令。

```
awk -F "\"*,\"*" '{ print $1 " " $2 }' accounts.csv > full_name.txt
```

输出文件:

```
# full_name.txt
Nadine RudkinCesaro SpearmanBryant WhewayStarlene TomaelloAbagael HawtonStacy LennardFrancis IacobettoAilina BurdonJoline DymickIrving OdhamsGisele PlutheroWoody HousdenTaite DennistounTerza MowdayVida CroomePerle RenodenRawley GrayhamKippar DeemingMargery DevereuFenelia Ivanitsa
```

在这里注册我的电子邮件列表。

**获取名字中包含字母 a 的帐户。**

输入文件:

```
# accounts.csv
Nadine,Rudkin,nrudkin0@wikia.com,Male,200.61.156.1972,
Cesaro,Spearman,cspearman1@blinklist.com,Female,113.65.62.713,
Bryant,Wheway,bwheway2@infoseek.co.jp,Male,12.232.111.1934,
Starlene,Tomaello,stomaello3@google.com.au,Female,110.137.45.1035,
Abagael,Hawton,ahawton4@nbcnews.com,Female,199.194.154.1186,
Stacy,Lennard,slennard5@goo.ne.jp,Male,16.221.204.647,
Francis,Iacobetto,fiacobetto6@paypal.com,Female,123.92.45.988,
Ailina,Burdon,aburdon7@163.com,Female,207.240.235.2549,
Joline,Dymick,jdymick8@bluehost.com,Female,78.207.170.5710,
Irving,Odhams,iodhams9@wisc.edu,Female,80.81.222.23011,
Gisele,Pluthero,gplutheroa@apache.org,Male,39.211.29.15612,
Woody,Housden,whousdenb@epa.gov,Male,204.249.180.14313,
Taite,Dennistoun,tdennistounc@list-manage.com,Male,195.157.252.17814,
Terza,Mowday,tmowdayd@marketwatch.com,Female,91.8.171.23115,
Vida,Croome,vcroomee@paypal.com,Male,119.6.60.16016,
Perle,Renoden,prenodenf@marriott.com,Female,198.239.236.9417,
Rawley,Grayham,rgrayhamg@samsung.com,Female,103.197.116.4618,
Kippar,Deeming,kdeemingh@tumblr.com,Male,60.73.105.18819,
Margery,Devereu,mdevereui@cnet.com,Female,44.159.148.23220,
Fenelia,Ivanitsa,fivanitsaj@uiuc.edu,Male,48.58.78.174
```

下面是命令。

```
awk '$1 ~ /^A/ {print $0}' accounts.csv > first_names_w_A.csv
```

输出:

```
# first_names_w_A.csv
Abagael,Hawton,ahawton4@nbcnews.com,Female,199.194.154.1186,Ailina,Burdon,aburdon7@163.com,Female,207.240.235.2549,
```

**提取系统进程 ID**

运行命令提取进程 ID。

```
ps aux | awk '{ print $2 }'
```

输出:

```
PID1234689101112131415161719202122232426272829303133...
```

在这里注册我的电子邮件列表。

## AWK 脚本

![](img/a8bf50aaef6a0f5f787faf0f74917396.png)

Photo by [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

AWK 不仅仅是一个文本提取工具，它还是一个成熟的脚本语言。您可以创建包含指令集的 awk 脚本文件。

**剧本的结构**

[该文件将包含](https://www.gnu.org/software/gawk/manual/gawk.html#BEGIN_002fEND)

```
BEGIN {}
```

块，它声明脚本的开始执行。您也可以使用

```
END {}
```

块，以确保在脚本结束时清理它。您可以使用 ARGC 和 ARGV 通过下面的语法[访问传入脚本的参数。](https://www.gnu.org/software/gawk/manual/html_node/ARGC-and-ARGV.html#ARGC-and-ARGV)

**值类型**

*   [数字学](https://www.gnu.org/software/gawk/manual/gawk.html#Constants)
*   [字符串](https://www.gnu.org/software/gawk/manual/gawk.html#Constants)
*   八进制数
*   十六进制数字
*   [正则表达式](https://www.gnu.org/software/gawk/manual/gawk.html#Constants)

**运行脚本中的功能**

您也可以使用下面的块声明一个函数。

```
function name(arg_0, ..., arg_n) {}
```

[脚本还有声明性语法块，如 for、while、do 等，这些是大多数脚本语言的标准。](https://www.gnu.org/software/gawk/manual/html_node/Statements.html#Statements)

**正则表达式**

您还可以使用调用正则表达式匹配

```
exp ~ /regexp/
```

**基本数据结构**

Awk 没有任何数据结构。您将使用的最重要的数据结构是数组。使用数组不需要任何花哨的声明。可以使用删除数组项目

```
delete array[i]
```

**正在评论**

注释就像 bash 和 python 等其他脚本语言一样。

```
# This is an awk script comment.
```

你可以阅读更多关于 awk following 的内容。

 [## GNU Awk 用户指南

### 这个文件记录了 awk，一个可以用来选择文件中特定记录并对其执行操作的程序…

www.gnu.org](https://www.gnu.org/software/gawk/manual/html_node/index.html#SEC_Contents) 

在这里注册我的电子邮件列表。

## 更多高级示例

下面是一些使用 awk 完成任务的高级脚本示例。

```
awk 'BEGIN { for(i=1;i<=6;i++) print "cube of", i, "is", i*i*i}'
```

这是输出。

```
cube of 1 is 1
cube of 2 is 8
cube of 3 is 27
cube of 4 is 64
cube of 5 is 125
cube of 6 is 216
```

你甚至可以创建一个进行斐波那契计算的脚本。

```
# Source: cubbi.com
# This program calculates the nth fibonacci number
# using alrogirhtm 1A: naive binary recursion
#
# compiled: N/A
# executed: awk -f f1a.awk n
#
# The BEGIN block in awk is the first thing to be executed
# This block prints out usage information if the wrong number of
# command-line arguments were supplied, or if the argument is a
# non-numeric string: for such strings, explicit string->number conversion
# (performed by adding zero in awk) turns them into zero.
BEGIN {
    if(ARGC==2 && 0+ARGV[1]!=0 || ARGV[1]=="0")
        fib_print(ARGV[1])
    else
        print "Usage: awk -f f1a.awk <n>"
}
# Function fib_print(n) prints the n'th Fibonacci number
function fib_print(n) {
    printf "%dth Fibonacci number is %d\n",n,f(n)
}
# Function f(n) handles the negative arguments: F(-n) = F(n)*(-1)^(n+1)
function f(n) {
    if(n<0)
        return n%2 ? fib(-n) : -fib(-n)
    else
        return fib(n)
}
# Naive binary recursion: F(n) = F(n-1) + F(n-2)
function fib(n) {
    return n<2 ? n : fib(n-1) + fib(n-2)
}
```

在这里注册我的电子邮件列表。

## 了解有关 AWK 命令的更多信息

您可以通过查看终端中的手册页来了解关于 AWK 命令的更多信息。

```
man awk
```

IT 和工程领域是快速发展的领域。跟不上意味着你将被落在后面。跟上的最好方法是保持最新的新闻和教育内容。[订阅免费电子邮件列表，将您的职业生涯提升 10 倍。](/subscribe/@dretechtips)

**加入我们吧，因为 50 多位想要快速提升职业生涯和知识基础的人已经注册了。**

**相关内容:**

*   [如何在 Linux 上安全删除文件？](http://How to SECURELY delete files in Linux?)
*   [如何在 Linux 上查找文件？](/geekculture/how-to-find-files-in-linux-6ed09a98c899)
*   [如何安全地连接到您的服务器？](/geekculture/ssh-securely-connect-to-your-servers-8895faab7083)