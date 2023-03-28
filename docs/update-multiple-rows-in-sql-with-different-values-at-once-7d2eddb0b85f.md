# 一次用不同的值更新 SQL 中的多行

> 原文：<https://medium.com/geekculture/update-multiple-rows-in-sql-with-different-values-at-once-7d2eddb0b85f?source=collection_archive---------0----------------------->

您是否遇到过需要用不同的值更新数据库中多行的问题？是的，这让我有点头疼。我将在这里展示如何在 NodeJS 中有效地解决这个问题。

![](img/bafa20569e5996ead194e8cf606a9767.png)

Image taken from [https://www.digitalocean.com/community/tutorials/how-to-manage-sql-database-cheat-sheet](https://www.digitalocean.com/community/tutorials/how-to-manage-sql-database-cheat-sheet)

想象一下，你在数据库中有很多条目。像一百万或更多。但是您想要将每个字段更改为不同值(假设您想要加密名字)。你将如何在 NodeJS 中做到这一点？嗯，一个解决办法是一个一个地做，但是这样效率不高，而且会花很多时间。但是我们可以试试。让我们在这个问题上使用 knex

```
knex.transaction(function(trx) {
    knex('users')
        .transactiong(trx)
        .select('id')
        .select('firstName')
        .then(async function(res) {
            for (let i = 0; i < res.length; i++) {
                const userData = res[i]; //here we have id and firstName
                const encryptedFirstName = encrypt(userData.firstName);trx('users')
                    .where('id', '=', userData.id)
                    .update({
                        firstName: encryptedFirstName
                    })
            }
        })
        .then(function() {
           trx.commit();
           process.exit(0);
        })
        .catch(function(err) {
           trx.rollback();
           process.exit(1);
        })
});
```

正如我所说的，这个解决方案对于低数据量是可以的。但是我们能更快吗？首先，我开始搜索了一下，发现了这个 SQL 解决方案:

```
UPDATE users
    SET firstName = (case when id = 1 then 'encryptedFirstName1'
                         when id = 2 then 'encryptedFirstName2'
                         when id = 3 then 'encryptedFirstName3'
                    end)
    WHERE id in (1, 2, 3);
```

看起来不错，不是吗？但是在我的第一份工作中，一位资深开发人员总是问我，我们能不能换一种方式来做？所以我开始想了又想，最后我开始建立一个解决方案。首先，我需要`id`和`firstName`。我们可以简单地说:

```
SELECT 1 as id, 'myFirstName1' as firstName;
```

好的，我们可以生成这个，没问题，但是我们需要一堆数据，怎么做呢？嗯，有一个叫做`UNION ALL`的命令将所有的选择连接在一起。

```
SELECT 1 as id, 'myFirstName1' as firstName
UNION ALL
SELECT 2 as id, 'myFirstName2' as firstName
UNION ALL
SELECT 3 as id, 'myFirstName3' as firstName
```

好了，现在我们生成了所有需要在`users`表中插入/更新的内容。但是怎么做呢？嗯，你知道吗，我们可以在`UPDATE`语句中使用`JOIN`？是的，我们可以。因此，我们将连接来自 select 语句的数据，并将我们的名字设置为新值，将 id 设置为我们的条件，如下所示

```
UPDATE users u
JOIN (
    SELECT 1 as id, 'myFirstName1' as firstName
    UNION ALL
    SELECT 2 as id, 'myFirstName2' as firstName
    UNION ALL
    SELECT 3 as id, 'myFirstName3' as firstName
) a
ON u.id = a.id SET u.firstName = a.firstName;
```

这不是比上面那个用`WHEN CASE`语句的方案好看多了吗？

现在，我们需要在 NodeJS 中实现它。我们将分批进行，因此需要定义我们的批量大小。让我们把它设置为 3000。我们需要两个循环；一个用于批处理循环，另一个用于加密批处理中的数据。

```
const BATCH_SIZE = 3000;
knex.transaction(function(trx) {
    knex('users')
        .transactiong(trx)
        .select('id')
        .select('firstName')
        .then(async function(res) {
            for(let i = 0; i < res.length: i+=BATCH_SIZE) {
                for(let j = 0; i < BATCH_SIZE; j++) {
                    const userData = res[i+j];
                }
            }
        })
        .then(function() {
           trx.commit();
           process.exit(0);
        })
        .catch(function(err) {
           trx.rollback();
           process.exit(1);
        })
});
```

现在，我们需要构建我们的查询。只觉得那个`JOIN`是动态的，其他的都是固定的，所以我们可以硬编码

```
let sql = 'UPDATE users u JOIN (';
for(let i = 0; i < res.length: i+=BATCH_SIZE) {
    let nestedSql = '';
    for(let j = 0; i < BATCH_SIZE; j++) {
        const userData = res[i+j];
    } sql += `${nestedSql}) a on u.id = a.id SET u.email = a.email;`;
    await trx.raw(sql);
}
```

好了，现在是“硬”的部分——嵌套 sql。除了第一个和最后一个，我们需要为每个查询添加`UNION ALL`。我们总是需要添加`SELECT`。但是最后一批呢？如果数据少于我们的批量怎么办？然后，我们的`userData`变量将是未定义的，我们需要退出。现在我们有了算法，我们只需要有正确的顺序。显然，首先要检查的是数据是否未定义。我们知道添加 select 语句不需要任何检查。所以，我们开始吧…

```
let sql = 'UPDATE users u JOIN (';
for(let i = 0; i < res.length; i+=BATCH_SIZE) {
    let nestedSql = '';
    for(let j = 0; i < BATCH_SIZE; j++) {
        const userData = res[i+j];

        if (!userData) {
            break;
        }

        if (j != 0) {
            nestedSql += 'UNION ALL ';
        }

        nestedSql += `SELECT \"${data.id}\" as id, \"${data.firstName}\" as firstName `;
    }

    sql += `${nestedSql}) a on u.id = a.id SET u.firstName = a.firstName;`;
    await trx.raw(sql);
}
```

现在我们可以运行它，它将执行 3000 行的批量更新。非常干净利落。

现在，继续提交这段代码，但是不要忘记请求 pull。如果你想从命令中创建 pull 请求，请务必查看我的另一篇文章，在这篇文章中我谈到了 GitHub CLI 工具。

[](https://golobitch.medium.com/so-lets-talk-about-github-cli-5155f299d88c) [## 那么，让我们来谈谈 GitHub CLI。

### 首先，我们需要安装它。在 macOS 上，事情很简单:

golobitch.medium.com](https://golobitch.medium.com/so-lets-talk-about-github-cli-5155f299d88c)