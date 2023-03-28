# 用 Prisma 设置时间刻度超表

> 原文：<https://medium.com/geekculture/set-up-a-timescaledb-hypertable-with-prisma-9550652cfe97?source=collection_archive---------9----------------------->

![](img/e94d993a675c367b8ed86800da930192.png)

Photo by [Andrey Novik](https://unsplash.com/@groove328?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/prisma?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

一个月前，我试图在 NodeJS + [Prisma](https://www.prisma.io/) 上设置 TimescaleDB。在 TimescaleDB [site](https://docs.timescale.com/timescaledb/latest/quick-start/node/) 上有一个教程，但是它使用了我不喜欢的类型。然后我发现了这个[指南](https://gist.github.com/janpio/2a425f22673f2de54469772f16af8118)，它非常有帮助，但是我发现了一些小错误。我花了一整天。我终于创建了一个超表。
今天，我试图再次创建一个超表格，我的时间被错误占满了。在我把它们整理好之后。我刚刚意识到我事先知道所有的陷阱，但是我把它们都忘了！所以我决定写这篇教程。

# 警告

> 据 Prisma 的工程主管 Jan Piotrowski 所说，“Prisma 不正式支持 TimescaleDB，这些非常手动的步骤使它工作，但是当然仍然存在不相关的事情出错的危险。”

1.  在 PostgresSQL 和 TimescaleDB 上，`""`(双引号)内的任何内容都*区分大小写*，而`''`(单引号)内的任何内容都被认为*转换成小写*。
2.  当你用`SELECT create_hypertable('[table]', '[field]');`命令创建一个超表时，它会创建一个索引，如果一个索引已经存在，创建失败。 *[* [*来源*](https://stackoverflow.com/questions/61205063/error-cannot-create-a-unique-index-without-the-column-date-time-used-in-part) *]*
    所以先删除索引，然后创建唯一键，因为 Prisma 至少需要一个唯一键或索引，然后创建一个超表。
3.  据我所知，`SELECT create_hypertable('[table]', '[field]');` 命令接受表名作为*区分大小写* e，但不接受字段名。请考虑用小写字母命名要用作超表键的字段名。

# 辅导的

**1** 。初始化 NodeJS 项目并安装 prisma。

```
mkdir timescale-prisma && cd timescale-prismanpm init -ynpm install prisma typescript ts-node @types/node --save-dev
```

**2** 。创建 Prisma 模式。

```
npx prisma init
```

这个命令创建了一个名为 *prisma* 的新目录，其中包含一个名为`schema.prisma`的文件和一个位于项目根目录下的`.env`文件。

**3** 。编辑`.env`文件以指向您的数据库。

```
DATABASE_URL=”postgresql://johndoe:randompassword@localhost:5432/mydb”
```

以上是:

*   用户名— `johndoe`
*   密码— `randompassword`
*   主机 URL — `localhost`
*   港口— `5432`(默认)
*   数据库名称— `mydb`

**4** 。然后编辑`schema.prisma`。它可能看起来像下面。

```
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}model Node {
  id        Int      *@id @default*(autoincrement())nodeName  String

  log       Log[]
}
model Log {
  id           Int      *@id @default*(autoincrement())loggedAt     DateTime *@default*(now())
  temperature  Float

  node         Node     *@relation*(fields: [nodeId], references: [id])
  nodeId       Int
}
```

你需要稍微修改一下。

```
datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}model Node {
  id        Int      @id *@default*(autoincrement())nodeName  String

  log       Log[]
}
model Log {
  id           Int      *@default*(autoincrement())logged_at    DateTime *@default*(now())
  temperature  Float

  node         Node     *@relation*(fields: [nodeId], references: [id])
  nodeId       Int *@@unique*([id,  logged_at])
}
```

按照说明，我将`loggedAt`字段重命名为`logged_at`，并将`@id`从`Log`、
的`id`字段中移除，然后添加`*@@unique*([id, logged_at])`作为唯一键。

> 我之所以将`id`和`logged_at`组合在一起，是因为我不认为只有时间戳是唯一的，因为来自其他节点的数据可能同时到达

**5** 。通过迁移生成`.sql`文件；`--create-only`表示您还不想对数据库进行更改。

```
npx prisma migrate dev --create-only
```

提示符将要求输入迁移名称，您可以键入任何名称。在这种情况下，我输入 *init。*

```
$ **npx prisma migrate dev --create-only**
Environment variables loaded from .env
Prisma schema loaded from db\schema.prisma
Datasource “db”: PostgreSQL database “mydb”, schema “public” at “localhost:5432”√ Enter a name for the new migration: … **init**
Prisma Migrate created the following migration without applying it 20210831083903_init
```

你会在`Prisma/migrations/`下的`20210831083903_init`文件夹中看到`migration.sql`。

```
-- CreateTable
CREATE TABLE "Node" (
    "id" SERIAL NOT NULL,
    "nodeName" TEXT NOT NULL,

    PRIMARY KEY ("id")
);

-- CreateTable
CREATE TABLE "Log" (
    "id" SERIAL NOT NULL,
    "logged_at" TIMESTAMP(3) NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "temperature" DOUBLE PRECISION NOT NULL,
);

-- CreateIndex
CREATE UNIQUE INDEX "Log.id_logged_at_unique" ON "Log"("id", "logged_at");

-- AddForeignKey
ALTER TABLE "Log" ADD FOREIGN KEY ("nodeId") REFERENCES "Node"("id") ON DELETE CASCADE ON UPDATE CASCADE;
```

**5。**在第一行插入`CREATE EXTENSION IF NOT EXISTS timescaledb CASCADE;` 以确保安装了 TimescaleDB 扩展。
在最后一行添加`SELECT create_hypertable('"Log"', 'logged_at');`，在`Log`表上创建超表。

**注 1** :如果你的表名包含大写字母，你需要把它们放在`""`之间，如上所述，但是如果你的表名是`log`，命令将是`SELECT create_hypertable('log', 'logged_at');`。
**注 2:** 看来我们用来创建超表的字段必须是`lower case`。因为我已经试过了`SELECT create_hypertable('"Log"', '"loggedAt"')`但是出错了。

最后，你的`migration.sql`看起来会像:

```
CREATE EXTENSION IF NOT EXISTS timescaledb CASCADE;
-- CreateTable
CREATE TABLE "Node" (
    "id" SERIAL NOT NULL,
    "nodeName" TEXT NOT NULL,

    PRIMARY KEY ("id")
);

-- CreateTable
CREATE TABLE "Log" (
    "id" SERIAL NOT NULL,
    "logged_at" TIMESTAMP(3) NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "temperature" DOUBLE PRECISION NOT NULL,
);

-- CreateIndex
CREATE UNIQUE INDEX "Log.id_logged_at_unique" ON "Log"("id", "logged_at");

-- AddForeignKey
ALTER TABLE "Log" ADD FOREIGN KEY ("nodeId") REFERENCES "Node"("id") ON DELETE CASCADE ON UPDATE CASCADE;SELECT create_hypertable('"Log"', 'logged_at');
```

**6** 。然后对数据库应用 sql 命令。

```
npx prisma migrate dev
```

提示会再次询问迁移名称，我输入 *hypertable* 。

```
$ **npx prisma migrate dev**
Environment variables loaded from .env
Prisma schema loaded from db\schema.prisma
Datasource “db”: PostgreSQL database “mydb”, schema “public” at “localhost:5432”The following migration(s) have been applied:migrations/
 └─ 20210831083903_hypertable/
 └─ migration.sql
√ Enter a name for the new migration: …**hypertable**The following migration(s) have been created and applied from new schema changes:migrations/
 └─ 20210831083958_hypertable/
 └─ migration.sqlYour database is now in sync with your schema.✔ Generated Prisma Client (2.30.2) to .\node_modules\[@prisma](http://twitter.com/prisma)\client in 181ms
```

搞定了。现在您的超表格已经准备好了。请参见 TimescaleDB [文档](https://docs.timescale.com/timescaledb/latest/quick-start/node/#insert_rows)如何向其中插入行并执行查询。