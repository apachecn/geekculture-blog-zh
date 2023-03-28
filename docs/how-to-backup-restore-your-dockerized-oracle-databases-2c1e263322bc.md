# 如何备份和恢复您的归档 Oracle 数据库

> 原文：<https://medium.com/geekculture/how-to-backup-restore-your-dockerized-oracle-databases-2c1e263322bc?source=collection_archive---------6----------------------->

![](img/a2a03c7aefce915cdcab4c38f3ac646a.png)

Photo by [B K](https://unsplash.com/@woolyart?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

最近，我正在处理一项关于 Oracle 数据库的工作任务，我必须备份我的工作数据库，以确保我不会浪费将数据填充到这些数据库中所花费的时间。但是我很难在网上找到一个来源，在一个 dockerized oracle 数据库的上下文中解释这一点。因此，经过一番挖掘，我能够让它工作，并想到写这篇短文。

我将使用 Oracle Database 19c Docker 映像，它是按照 Oracle Github[](https://github.com/oracle/docker-images/tree/main/OracleDatabase/SingleInstance)****，*** 中的说明在本地构建的，我假设您已经在本地/远程机器上设置了数据库。*

*首先，您应该使用下面的命令使用`docker exec`在 docker 容器内部进行 bash。*

```
*docker exec -it <NAME-OF-DOCKER-CONTAINER>*
```

*在其中，您应该使用`sqlplus`作为`sysdba`用户登录到您的数据库实例。*

```
*sqlplus / as sysdba@<NAME-OF-YOUR-PDB/CDB>*
```

*首先，我们应该在我们的 PDB 中创建一个`DIRECTORY`数据库对象，它将本地(docker 容器的)文件系统中的一个位置映射到我们的数据库。*

```
*ALTER SESSION SET CONTAINER=<PDB-NAME>;CREATE OR REPLACE DIRECTORY EXP_DIR AS '/home/oracle';*
```

*然后向您的用户授予对创建的目录对象的读/写权限。*

```
*GRANT READ, WRITE ON DIRECTORY EXP_DIR TO <USERNAME>;*
```

*并授予导出和导入权限。*

```
*GRANT datapump_exp_full_database TO <USERNAME>;
GRANT datapump_imp_full_database TO <USERNAME>;*
```

*现在，在数据库端完成了必要的设置。键入`exit;`,再次退出 bash 终端。*

*现在您可以使用内置的`expdp`实用程序来导出您的数据库；*

```
*expdp <ORIGINAL-DATABASE-USERNAME>/<PASSWORD>@<PDB-NAME> schemas=<ORIGINAL-DATABASE-USERNAME> directory=EXP_DIR dumpfile=db1.dmp logfile=db1.log*
```

*例如，我有一个名为 AMDB 的用户/数据库，在 PDB 的密码是“test123”，“ORCL”。所以我的导出命令看起来像这样。*

```
*expdp AMDB/test123@ORCL schemas=AMDB directory=EXP_DIR dumpfile=db1.dmp logfile=db1.log*
```

*如果所有给定的设置都正确，该命令将触发数据库导出，这将需要一些时间。*

*该命令完成后，您将看到映射目录中的两个文件，在本例中是`/home/oracle`目录。*

*现在，您的数据库已成功备份，这两个文件可用于恢复您的数据库。*

*现在让我们来看看这个过程。*

*为此，您可以使用 oracle 提供的`impdp`实用程序。*

```
*impdp system/<SYSTEM-PASSWORD>@<PDB-NAME> schemas=<ORIGNAL-USERNAME>  remap_schema=<ORIGNAL-USERNAME>:<NEW-USERNAME-TO-BE-CREATED> directory=EXP_DIR dumpfile=db1.dmp logfile=db1.log*
```

*就我而言，*

```
*impdp system/password@ORCL schemas=AMDB  remap_schema=AMDB:AMDB_BKUP directory=EXP_DIR dumpfile=db1.dmp logfile=db1.log*
```

*这也将触发一组新的过程，在此过程结束时，您的新数据库/用户将以您提供的名称 AMDB_BKUP 创建。*

*就像这样，您可以使用 Oracle 内部提供的工具轻松备份和恢复您的数据库！*

*![](img/6432b3e78203e672c61345122daa5285.png)*

*Photo by [Sigmund](https://unsplash.com/@sigmund?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)*

*我希望这篇文章对你有用，我会列出一些我在写这篇文章时喜欢的资源。如果你想了解更多关于 Java、API、微服务和数据库的内容，请关注我们！干杯。*

*[](https://www.linkedin.com/in/rashmin95/) [## Ravindu Rashmin -软件工程师- WSO2 | LinkedIn

### 在世界上最大的职业社区 LinkedIn 上查看 Ravindu Rashmin 的个人资料。Ravindu 列出了 7 个工作…

www.linkedin.com](https://www.linkedin.com/in/rashmin95/) 

资源—

 [## 创建目录

### 目的使用 CREATE DIRECTORY 语句创建目录对象。目录对象为…指定别名

docs.oracle.com](https://docs.oracle.com/cd/B19306_01/server.102/b14200/statements_5007.htm) [](https://oracle-base.com/articles/10g/oracle-data-pump-10g) [## Oracle 数据库 10g、11g、12c、18c、19c、21c 中的 Oracle 数据泵(expdp、impdp)

### Oracle 数据泵是一种更新、更快、更灵活的替代工具，可以替代以前使用的“exp”和“imp”工具

oracle-base.com](https://oracle-base.com/articles/10g/oracle-data-pump-10g) [](https://docs.oracle.com/en/database/oracle/oracle-database/19/sutil/oracle-data-pump-export-utility.html) [## 公用事业

### 注意:有几个系统方案无法导出，因为它们不是用户方案。它们包含 Oracle 管理的数据…

docs.oracle.com](https://docs.oracle.com/en/database/oracle/oracle-database/19/sutil/oracle-data-pump-export-utility.html)*