# 锁定高级设计

> 原文：<https://medium.com/geekculture/lock-high-level-design-3e02bbb8eb7f?source=collection_archive---------26----------------------->

# **简介 2021**

这个文档最初是我自己写的，大约在 2014 年实现的。

*   注意，所有系统都是一堆 Web 服务(不是微服务！).
*   另外，请注意，每个微服务有一个共享数据库，而不是数据库/模式。

基本思想如下:有一些离线工作的 web 服务。为了确保数据的一致性，任何人都不应该使用它的数据。当这样的 web 服务开始工作时，需要写锁。为了简单起见，我们假设此时不需要读锁(否则，写锁就不能被获取)。因此，当写锁没有被释放时，任何人都不能获得这个 web 服务的读锁，这样的请求将会失败。当这样的 web 服务将完成它的工作时，写锁将被释放，每个人都可以请求读锁并使用这些数据(可选地，我们还可以限制并发读锁的数量)。

第一个版本基于 MySql 和`select for update` 特性。它写于 2009 年左右，不是我自己写的。决定重写实现有两个基本原因:

*   我们已经使用了 MySql 和 postgresh，我们想把所有东西都移植到 postgresh。
*   如果 Web 服务被`kill -9` *停止，对 DB 表*(不仅仅是数据)的锁定不会释放，需要一些手动干预。
*   *Minor* :用半静态表去掉左连接，在内存中进行连接，可以提高性能。

如果我今天要重写这个系统，我会使用[亚马逊 SNS](https://aws.amazon.com/sns/) 或 *RabbitMQ* 甚至 *Kafka* (回放*所有事件的能力可以这样利用)。*

# 初始化

当应用程序启动时，有两件事情要处理— *releaseAllLocks* (如果我们在中间杀死应用程序，那么锁被获取而没有被释放，但是没有作业正在进行)和*refreshlockpletions*(每个锁的许可数量)。

*   *releaseAllLocks* —当应用程序刚刚启动时，数据库中不应存在以前运行的锁。
    所以我们要通过 *app_id* 解锁所有 look。如果我们不这样做，锁将继续丰富。只需删除这些行。(由 *app_id 识别)*。
*   *refreshlockpletions*—我们应该更新被覆盖的许可值的缓存(来自**Locking _ permissions)。**有关更多详细信息，请参见“*锁定许可缓存*”部分。

# 锁定获取

这是 API 中的第一个主要方法。此方法尝试获取锁。如果失败，`0L`将被退回，如果成功，图章将被退回。

`*public long tryLocks(Set<Lock> lock)*`

*注意:*也是 *app_id* 隐式传递(将在应用`init` 阶段设置)。但是，不应该用这种方法。

Lock 是一个简单的类(不是所讨论的`enum` ),如果是读或写锁，它保存*枚举名称*和*布尔指示*。

典型 WebService2 持有`Lock.WS2_WRITE`和`Lock.WS1_READ`(这就是为什么它接收*组锁*而不是单个锁)。

*   **注 2021:** 我们希望能够动态地*为系统添加网络服务和锁。所以，锁可以是 Java 的包含所有锁的`enum`类。我做了一些解决方案来动态地添加实例到 T5，但是这只是一个大的黑客行为，可以在小的 JDK 更新上达到收支平衡。拼图游戏项目将会在 JDK 上映，所以对我来说很清楚，这种黑客行为会产生大量的技术债务。*

*它返回将作为参数传递给锁释放的戳。将返回特殊值零，表示无法获取访问权限。*

***注 2021** :在实际实现中，我也实现了一些简单的重试机制，作为`tryLocks()`方法的包装。更多详情见下面的*“事务隔离”*部分。*

# *算法(由*锁名*提取):*

1.  ***用** `**SERIALIAZABLE**` **隔离开始事务。***

**注意:* `SERIALIAZABLE` 交易在 8.4 版本中不工作，需要升级到 9.1 以上版本。详情参见下面的*“事务隔离”*部分。*

*2.收集所有锁名(不考虑读/写模式)。*

*3.按锁名选择数据库中的所有锁。*

*4.对于锁中的每个锁:*

*a)计算 *writeLockInUse* —使用中的写锁数量(从数据库结果中)；
b)计算 *readLockInUse* —使用中的读锁数量(从数据库结果中)；
c)计算*许可*(从使用乐观读锁的缓存)。*

*使用乐观读锁(`tryOptimisticRead()`方法)。*

*大多数情况下它会成功。在缓存更新的偶然事件中，使用常规的阻塞读锁。参见`StampedLock` [的 JavaDoc 中的`distanceFromOrigin()`示例 https://docs . Oracle . com/javase/8/docs/API/Java/util/concurrent/locks/stampedlock . html](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/locks/StampedLock.html)*

*1.1.如果是写锁定:*

*a)如果*readLockInUse*0 返回 0L；*

*b) *如果 writeLockInUse* + 1 > *允许*返回 0L；*

*c)否则(没有使用读锁并且*写锁使用* + 1 < = *许可*，对于许可=1 这简化为*写锁使用< =0* 这意味着没有使用写锁)*

*根据数据库数据检查我们是否应该插入或更新数据库中的行，将此信息存储在 *readLockToUpdate、writeLockToUpdate、readLockToInsert、writeLockToInsert 中。**

*1.2.如果是读锁:
a)如果*写锁使用*0 返回 0L；
b)如果*readlock inuse*1>允许返回 0L；*

*c)否则(不使用写锁，读锁使用*+1<=*许可*，对于许可=1，这简化为*读锁使用< =0* ，这意味着不使用写锁)**

*2.如果我们在这里，那就是我们成功地验证了所有的锁都可以被获取。*

*3.创建最终 SQL。使用 *batchUpdate()* 批量执行。*

**注意:*不要求戳记是密码安全的(即有效戳记可以是可猜测的)。更多详情参见*“数据库表”*部分。*

# *释放锁*

*这是 API 中的第二个主要方法。此方法释放获得的锁。如果失败，将返回 0L，如果成功，将返回 stamp。*

```
**public void releaseLocks(long stamp)**
```

# *算法(按 app_id 删除，戳):*

1.  *删除这些行。(由 *app_id，stamp 识别)*。
    `JdbcTemplate’s *update*` *()* 方法返回受影响的`*numberOfRows*`。*
2.  *如果`*numberOfRows*` ==0 抛出`IllegalMonitorStateException.` 没有找到行，则传递无效戳。*

***注意:**因为 API 接收为参数戳，所以在锁获取中不存在(除非有人会猜测，否则可以忽略)，所以 *releaseLocks* ()不能与 *tryLocks* 混淆。*

*   *如果我们用不同的 stamp(和 app_id)对 *releaseLocks()* 进行两次并发运行，这里没有问题，它们使用表的不同部分。*
*   *如果 stamp 是相同的(并且我们忽略与*相同 stamp* 的组合，但是*不同的 app_id* 将在同一时间范围内 *)* 在不同的机器上创建，由于行排他锁，一个事务将首先成功完成，而另一个事务将不删除任何行并且失败。*

***注:***

*   *只有当我们只有一个 delete 语句时，上面的分析才是正确的。如果我们有更多的 SQL 语句，它就可能被打破。*

# *锁定许可缓存*

*基本思想是将所有表格内容保存在`ConcurrentHashMap`中。在应用程序初始化中加载它。还有*后台线程*会定期刷新这个缓存。这样，如果我们在数据库中更改配置，所有应用程序将在一段时间后看到这一变化，不需要重新启动。*

*`*public void refreshLockPermits()*`*

***注:***

1.  *这些配置应该存储在 DB 中，而不是属性文件中，因为我们希望它在所有 web 服务中完全相同。*
2.  *在从数据库的每个选择中，缓存的使用优于对**Locking _ permissions**进行左连接(这是在以前的实现中)。我们需要做两个左连接(计算读锁和写锁的许可)。使用缓存会更快。*

*在`ConcurrentHashMap`更新中几乎没有什么微妙之处。*

*1.简单的方法是调用 `cache.clear();cache.putAll(dataFromDB);`
这种方法有两个问题:*

*a)这不是原子更新。在`clear()` 完成和开始`putAll(`之间有一段时间，我们有空的缓存。我们的锁机制将在错误的许可值下工作。*

*b) `clear()`和 `putAll()`本身不是原子的。引用自 ConcurrentHashMap Javadoc:*

> *对于 putAll 和 clear 之类的聚合操作，并发检索可能只反映某些条目的插入或删除…请记住，聚合状态方法(包括 size、isEmpty 和 containsValue)的结果通常只有在映射没有在其他线程中进行并发更新时才有用。否则，这些方法的结果反映的瞬态可能足以用于监测或估计目的，但不适用于程序控制。*

*[https://docs . Oracle . com/javase/8/docs/API/Java/util/concurrent/concurrent hashmap . html](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentHashMap.html)*

*因此，我们需要使用一些外部同步机制来更新地图，更重要的是，读取地图。*

*2.映射中的条目不是独立的。如果数据库中的读锁被更改，我们应该自动更新读锁和写锁。*

# *算法(为**锁定 _ 许可**提取全部):*

*1.使用`StampedLock`进行缓存的外部同步。*

*2.大部分时间 DB 没有变化。
如果没有变化——不需要更新高速缓存，因此首先让我们识别是否是这种情况。
如果是，我们不会更新缓存，锁获取代码也不会阻塞。如果有变化，我们会阻止。*

*a)将来自 DB 的数据存储在本地映射中(与我们的缓存具有相同的结构)；*

*b)使用阻塞读锁来访问我们的高速缓存；*

*c)比较两个映射，如果它们等于返回(释放读锁定)；*

*d)如果它们不等于将锁升级为阻塞写锁(参见 JavaDoc 的`moveIfAtOrigin()`方法作为示例)；*

*e) `cache.clear(); cache.putAll(localMap);`(注意，这是在写锁定下完成的)*

*f)释放写锁定；*

# *数据库表*

***锁定**(主表)*

*这是锁定机制中的主表。所有正在使用的锁都存储在这个表中。DB 中的每一行都代表锁。*

*`*Lock_Name*` —例如锁的名称(`WS1`)；*

*`*Mode*` — `null` (未锁定)，`R` —读取，`W`—写入锁定；`WS1`锁定在读模式这里会有`R`，在写模式会有`W`；*

*`*App_id*` —标识哪个应用程序/web 服务创建/管理此锁。获取锁时被忽略。*

*`*Stamp*` —标识所获得的锁的集合的标记；*

*戳记将使用有限表示，并且不是密码安全的(即，有效的戳记可能是可猜测的)。`System.currentTimeMillis()`将被使用。这与 StampedLock 接口一致。引自 it's JavaDoc:*

> *戳记使用有限的表示，并且不是密码安全的(即，有效的戳记可能是可猜测的)。连续运行一年后(不早于一年),印花税可以回收。超过此期限未使用或未验证的图章可能无法正确验证。StampedLocks 是可序列化的，但总是反序列化为初始解锁状态，因此它们对远程锁定没有用。*

*[https://docs . Oracle . com/javase/8/docs/API/Java/util/concurrent/locks/stampedlock . html](https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/locks/StampedLock.html)*

*`*Created*` —仅供参考，何时获得锁定；*

*主键是`(lockName, mode, stamp)`。*

*这里不应该定义任何索引(在任何给定的时间内，该表都不应该超过 100 行，因此 100 行可能适合一个磁盘页，并且没有计划可以超过顺序获取 1 个磁盘页)。*

***锁定 _ 许可**(可选)*

*默认情况下，写锁是排他锁(1 个允许)，如果没有使用写锁，则可以使用 1 个读锁。对于特定的锁，您可以覆盖此表中的这些默认值。*

*`*Lock_Name*` —锁的名称；*

*`*Mode*` — `null` (未锁定)，`R` —读取，`W`—写入锁定；*

*`*Permits*` —许可数量。应该是整数> =1。*

**注意:*这些配置应该存储在数据库中，而不是属性文件中，因为我们希望它在所有应用程序中完全相同。*

*主键是`(lockName, mode)`。*

*此处不应定义任何索引(对于主键，回归会自动创建索引)。*

# *事务隔离*

*锁定获取将通过`SERIALIAZABLE` 隔离完成。所有其他的东西都会被处理掉*

**默认* ( `READ_COMMITED`)。*

*`SERIALIAZABLE` 交易无法在 8.4 版本中运行，需要升级到 9.1 以上版本。详见[http://stack overflow . com/questions/12824217/isolation-level-serializable-in-spring-JDBC](http://stackoverflow.com/questions/12824217/isolation-level-serializable-in-spring-jdbc)。*

*让我们看看一些国会文件:*

> *错误:由于并发更新，无法序列化访问。
> 
> 当应用程序收到这个错误消息时，它应该中止当前的事务，并从头开始重试整个事务。*

*[https://www . PostgreSQL . org/docs/current/static/transaction-iso . html](https://www.postgresql.org/docs/current/static/transaction-iso.html):*

*因此**应该开发锁客户端的重试机制**。

另引:*

> *`Repeatable Read`模式提供了一个严格的保证，即每个事务看到一个完全稳定的数据库视图。然而，这种观点不一定总是与同一级别的并发事务的一些串行(一次一个)执行相一致。*
> 
> ***注意:**在 PostgreSQL 版本 9.1 之前，对`Serializable` 事务隔离级别的请求提供了与这里描述的完全相同的行为。*【上图】**

*[https://www . PostgreSQL . org/docs/9.4/static/applevel-consistency . html](https://www.postgresql.org/docs/9.4/static/applevel-consistency.html)*

****注 2021:*** 可以说**在 9.1 版之前没有“真正的”** `***Serializable***` **事务隔离级别。***

*另一段引用自*

> *如果将`Serializable` 事务隔离级别用于需要一致数据视图的所有写入和读取，则不需要其他努力来确保一致性。
> 
> 当使用这种技术时，如果应用软件通过一个自动重试因串行化失败而回滚的事务的框架，它将避免给应用程序员造成不必要的负担。将 default_transaction_isolation 设置为 serializable 可能是个好主意。*【在数据源级别】**

*[https://www . PostgreSQL . org/docs/9.4/static/applevel-consistency . html](https://www.postgresql.org/docs/9.4/static/applevel-consistency.html)*

*实际上在我们的例子中没有，因为我放松了释放锁的要求，**它不应该是可序列化的。***