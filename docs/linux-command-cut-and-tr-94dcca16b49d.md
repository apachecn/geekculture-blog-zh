# Linux 命令:cut 和 tr

> 原文：<https://medium.com/geekculture/linux-command-cut-and-tr-94dcca16b49d?source=collection_archive---------10----------------------->

## “cut”和“tr”命令行实用程序概述

![](img/7278359729ef3e0e954a00155be53ffc.png)

Photo by [Andre Taissin](https://unsplash.com/@andretaissin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## Linux 命令:剪切

**cut** 命令是一个命令行实用程序，允许我们**剪切掉指定文件或管道数据的**部分，并将结果打印到标准输出。

```
>> man cut

cut OPTION... [FILE]...

# Options
-c, --characters=LIST
-d, --delimiter=DELIM   
-f, --fields=LIST
..
..
```

下面是一个虚拟文本文件:让我们看看如何操作下面的文本文件，根据我们的需要打印输出。

```
# dummy.txt

06:38:28.077150666: Error Package management process launched in container 06:38:28.077150666,090aad374a0a,nginx,root
06:38:33.058263010: Error Package management process launched in container 06:38:33.058263010,090aad374a0a,nginx,root
06:38:38.068693625: Error Package management process launched in container 06:38:38.068693625,090aad374a0a,nginx,root
06:38:43.066159360: Error Package management process launched in container 06:38:43.066159360,090aad374a0a,nginx,root
06:38:48.059792139: Error Package management process launched in container 06:38:48.059792139,090aad374a0a,nginx,root
06:38:53.063328933: Error Package management process launched in container 06:38:53.063328933,090aad374a0a,nginx,root
```

## 按字符范围打印

打印出一定字符范围内的输出:

```
# range 1-5
>> cut -c 1-5 dummy.txt

06:38
06:38
06:38
06:38
06:38
06:38

# range 20-40
>> cut -c 20-40 dummy.txt

 Error Package manage
 Error Package manage
 Error Package manage
 Error Package manage
 Error Package manage
 Error Package manage

# range 76-end
>> cut -c 76- dummy.txt

06:38:28.077150666,090aad374a0a,nginx,root
06:38:33.058263010,090aad374a0a,nginx,root
06:38:38.068693625,090aad374a0a,nginx,root
06:38:43.066159360,090aad374a0a,nginx,root
06:38:48.059792139,090aad374a0a,nginx,root
06:38:53.063328933,090aad374a0a,nginx,root 
```

## 按字段打印

假设，我们想根据字段从下面的文件中提取数据。

```
# dummy1.txt 
NAME EMAIL PHONE ADDRESS
devid devid@xyz.com 0897663232 tokyo,japan
harry harry@xyz.com 0232323232 miyazaki,japan
jane jane@xyz.com 0323213122 osaka,japan
```

我们必须使用“ ***-d =分隔符*** ”选项来分隔每个字段(可以是字符，默认为制表符)。然后我们必须指定要打印的字段编号。

```
-d, --delimiter=DELIM   
-f, --fields=LIST 

>> cut -d ' ' -f1
```

在下面的演示中，我们使用空格(“”)作为分隔符。

```
>> cut -d ' ' -f1 dummy1.txt 
NAME
devid
harry
jane

>> cut -d ' ' -f2 dummy1.txt 
EMAIL
devid@xyz.com
harry@xyz.com
jane@xyz.com

# Print out multiple field

>> cut -d ' ' -f1,3 dummy1.txt 
NAME PHONE
devid 0897663232
harry 0232323232
jane 0323213122 
```

如前所述，分隔符可以是字符，在下面的演示中，我们使用逗号(，)作为分隔符:

```
>> echo "jane,jane@xyz,236473264,osaka" | cut -d ','  -f1
jane

>> echo "jane,jane@xyz,236473264,osaka" | cut -d ','  -f2
jane@xyz

>> echo "jane,jane@xyz,236473264,osaka" | cut -d ','  -f3
236473264
```

## Linux 命令:tr

**tr** 命令—从标准输入中翻译、挤压和/或删除**字符**，写入标准输出。

```
>> man tr

tr [OPTION]... SET1 [SET2]

# Options
-c, --complement -- use the complement of SET1
-d, --delete -- delete characters in SET1, do not translate
-s, --squeeze-repeats -- replace each sequence of a repeated character 
                         that is listed in the last specified SET, 
                         with a single occurrence of that character
..
..
```

## 替换字符:

```
>> echo "Hello World" | tr 'H' 'h'
hello World

>> echo "Hello World" | tr 'Ho' 'KK'
KellK WKrld
```

## 删除字符:

```
>> echo "Hello World" | tr -d 'Ho'  
ell Wrld

# complement the delete
>> echo "Hello World" | tr -cd 'Hd\n' 
Hd

>> echo "Hello World 12345 " | tr -cd [:digit:]
12345

>> echo "Hello World 12345 " | tr -cd [:alpha:]
HelloWorld
```

## 挤压字符

```
>> echo "HHHHHHHHello Worrrrrrrrrldddddddddddddddddd" | tr -s 'Hord' 
Hello World

>> echo "Hello World" | tr -s [:lower:] [:upper:]
HELO WORLD
```

## 🖥所有关于 Linux 的文章👇

![Md Shamim](img/b46bdc53005abde6c6cb3e8ff0c200c3.png)

[Md 沙米姆](/@shamimice03?source=post_page-----94dcca16b49d--------------------------------)

## 所有关于 Linux 的文章

[View list](/@shamimice03/list/all-articles-on-linux-1339e15e3304?source=post_page-----94dcca16b49d--------------------------------)12 stories![](img/259cf1a3ab76526a3f714f7cbaffac3d.png)![](img/985a8b8ce1f1090a033d94ee7ac5f4fd.png)![](img/d917609dcb8b45812e60ccd8bf2ed277.png)