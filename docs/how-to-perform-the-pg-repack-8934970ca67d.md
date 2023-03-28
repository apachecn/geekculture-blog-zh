# 如何执行 pg_repack？

> 原文：<https://medium.com/geekculture/how-to-perform-the-pg-repack-8934970ca67d?source=collection_archive---------14----------------------->

在这里，我们将看到执行 pg_rpack 的一步一步

![](img/a4e7bd39ff9855bb0973f1f20190c1a4.png)

S-1:

让我们连接服务器，为此活动创建示例数据库

```
[ec2-user@ip-172-31-34-18 ~]$ psql -h database-1.cdb6vnednjo5.ap-south-1.rds.amazonaws.com -U postgres -d postgres
Password for user postgres:
psql (12.12, server 12.7)
SSL connection (protocol: TLSv1.2, cipher: ECDHE-RSA-AES256-GCM-SHA384, bits: 256, compression: off)
Type "help" for help.postgres=> create database mynotesoracledba;
CREATE DATABASE 
```

S-2:

在数据库中创建扩展，我们将在这里执行 pg_repack。

```
create extension pg_repack;
```

S-3:

验证安装的扩展

```
mynotesoracledba=> \dx
                                  List of installed extensions
   Name    | Version |   Schema   |                         Description
-----------+---------+------------+--------------------------------------------------------------
 pg_repack | 1.4.5   | public     | Reorganize tables in PostgreSQL databases with minimal locks
 plpgsql   | 1.0     | pg_catalog | PL/pgSQL procedural language
```

S-4:

创建示例表对象

```
CREATE TABLE burritos (
id SERIAL UNIQUE NOT NULL primary key,
title VARCHAR(10) NOT NULL,
toppings TEXT NOT NULL,
thoughts TEXT,
code VARCHAR(4) NOT NULL,
UNIQUE (title, toppings)
);
```

禁用此表的自动真空设置

```
ALTER TABLE burritos SET (autovacuum_enabled = false, toast.autovacuum_enabled = false);
```

S-5:

让我们对表执行更新

```
UPDATE burritos SET thoughts = md5(random()::text) where id < 10000;
UPDATE burritos SET thoughts = md5(random()::text) WHERE id between 10000 and 20000;
UPDATE burritos SET thoughts = md5(random()::text) WHERE id between 800000 and 900000;
UPDATE burritos SET toppings = md5(random()::text) WHERE id between 600000 and 700000;
```

S-6:

通过 **pg_stat_user_tables** 验证死元组百分比

```
SELECT relname, n_dead_tup, n_live_tup, 100*n_dead_tup/n_live_tup AS percent, left(last_vacuum::text,16) AS last_vacuum, left(last_autovacuum::text,16) AS last_autovacuum, last_analyze, left(last_autoanalyze::text,16) AS last_autoanalyze, now() FROM pg_stat_user_tables ;
```

注意:这里的百分比列显示死元组百分比的结果

S-7:

验证 pg_repack 安装的版本

```
[ec2-user@ip-172-31-34-18 ~]$ cd pg_repack-1.4.5
[ec2-user@ip-172-31-34-18 pg_repack-1.4.5]$ cd bin
[ec2-user@ip-172-31-34-18 bin]$ ./pg_repack --version
pg_repack 1.4.5
```

S-7:

pg_repack 有很多选项，可以随意查看，但我要开始排练了

```
./pg_repack -h database-1.cdb6vnednjo5.ap-south-1.rds.amazonaws.com -U postgres -d mynotesoracledba --dry-run --table burritos
INFO: Dry run enabled, not executing repack
Password:
ERROR: pg_repack failed with error: You must be a superuser to use pg_repack
```

在上面的例子中，我们得到的错误是由于超级用户权限不足，为了避免这个错误，我们必须使用-k 选项

```
[ec2-user@ip-172-31-34-18 bin]$ **./pg_repack -h database-1.cdb6vnednjo5.ap-south-1.rds.amazonaws.com -U postgres -d mynotesoracledba --dry-run --table burritos -k**
INFO: Dry run enabled, not executing repack
Password:
INFO: repacking table "public.burritos"
```

如果排练没有出现任何错误，那么我们就可以继续表演

S-8:

我们可以开始 pg_repack 了

```
[ec2-user@ip-172-31-34-18 bin]$ **./pg_repack -h database-1.cdb6vnednjo5.ap-south-1.rds.amazonaws.com -U postgres -d mynotesoracledba  --table burritos -k**
Password:
INFO: repacking table "public.burritos"
```

S-9:

要监控 pg_repack 会话，请使用 **pg_stat_activity** 视图或 Performance Insights 控制台。

```
 select * from pg_stat_activity where application_name='pg_repack'
```