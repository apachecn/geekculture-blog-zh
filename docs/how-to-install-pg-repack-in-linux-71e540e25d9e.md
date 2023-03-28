# 如何在 Linux 中安装 Pg_repack

> 原文：<https://medium.com/geekculture/how-to-install-pg-repack-in-linux-71e540e25d9e?source=collection_archive---------5----------------------->

在这里，我们将看到一步一步的 pg_repack 安装

![](img/8fdef621856966499c83d2bd4c7eaad8.png)

什么是 Pg_repack？

*   Pg_repack 是一个扩展，由几个作者作为开源项目创建和维护。
*   它消除了表和索引的膨胀，并可以选择恢复聚集索引的物理顺序。

**Pg_repack 安装步骤**

s-1:

要验证操作系统版本

```
[ec2-user@ip-172–31–13–65 ~]$ cat /etc/os-release
```

s-2:

以 root 用户身份切换并更新现有软件包

```
[root@ip-172-22-13-65 ~]# yum update -y
```

s-3:

Wget 是一个免费的开源软件包，用于使用 HTTP、HTTPS 和 FTP 互联网协议获取文件。

```
[root@ip-172-22-13-65 ~]# yum install wget
```

s-4:

切换到 ec2-user 并执行下面的依赖库

*   Readline-devel =开发使用 Readline 库的程序所需的文件，
*   OpenSSL=支持加密的工具包。

```
sudo yum install readline-devel openssl-devel
```

s-5:

PostgreSQL-devel 包是为 PostgreSQL 开发头文件和库而设计的。

```
sudo yum install postgresql-devel
```

s-6:

GNU 编译器集合(GCC)是 C、C++、Objective-C、Fortran、Ada、Go 和 D 编程语言的编译器和库的集合。许多开源项目包括 GNU 工具和 Linux 内核都是用 GCC 编译的。

```
sudo yum install gcc
```

s-7:

安装 PostgreSQL 客户端-服务器 rpm，我们必须安装基于 PostgreSQL 开发工具的 PostgreSQL 包

```
wget [http://mirror.centos.org/centos/7/updates/x86_64/Packages/postgresql-static-9.2.24-8.el7_9.x86_64.rpm](http://mirror.centos.org/centos/7/updates/x86_64/Packages/postgresql-static-9.2.24-8.el7_9.x86_64.rpm)
```

s-8:

使用下面的命令验证下载的包

```
ls -lrth
```

s-9:

安装 PostgreSQL 下载的 rpm 包

```
sudo rpm -i postgresql-static-9.2.24-7.el7_9.x86_64.rpm
```

s-10:

根据支持建议下载 pg_repack 1.4.5，PostgreSQL 版本 12 将仅支持 pg_repack 1.4.5，以防如果我们安装 pg_repack 最新版本 1.4.7，由于 pg_repack 版本不兼容，将无法执行 pg_repack

```
wget [http://api.pgxn.org/dist/pg_repack/1.4.5/pg_repack-1.4.5.zip](http://api.pgxn.org/dist/pg_repack/1.4.5/pg_repack-1.4.5.zip)
```

s-11:

解压缩下载的 pg_repack zip 文件

```
unzip pg_repack-1.4.5.zip
```

s-12:

一旦我们解压了 pg_repack，就进入下面的目录

```
cd pg_repack-1.4.5
```

s-13:

执行 **make** 命令，确保无错误报告。

```
make
```

s-14:

要验证 pg_repack 版本

```
cd bin./pg_repack --version
```