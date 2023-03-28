# ORM:劳动分工的深化

> 原文：<https://medium.com/geekculture/orm-the-deepening-division-of-labor-ebe39dfc167b?source=collection_archive---------16----------------------->

在这个故事中，我将概述 ORM 框架的历史，以及为什么我认为它们最终会失败。我将使用 Java 和 Python 中的具体例子。

引用亚当·斯密的话:

> 劳动生产力的最大提高，以及这种提高所涉及或应用的技能、技巧和判断力的最大部分，似乎都是劳动分工的结果。

第一章，第 7 页——国富论(1776)——第一册

## 另请参见:

[全栈:劳动分工的深化](https://alex-ber.medium.com/full-stack-the-deepening-division-of-labor-labor-5ff8b716c636)

让我们从头开始 ORM 框架要解决什么问题。

# 结构化查询语言

SQL 是一种**领域特定语言**，用于编程，设计用于管理保存在**关系数据库**管理系统(RDBMS)中的数据，或者用于关系数据流管理系统(RDSMS)中的流处理。它在处理结构化数据，即包含实体和变量之间关系的数据时特别有用。

SQL 最初基于**关系代数**和元组关系演算，由多种类型的语句组成……SQL 是最早使用 Edgar F. Codd 关系模型的商业语言之一。他在 1970 年发表了一篇颇具影响力的论文《大型共享数据库的数据关系模型》，对该模型进行了描述。尽管没有完全遵循 Codd 所描述的关系模型，它还是成为了使用最广泛的数据库语言。

SQL 在 1986 年成为美国国家标准协会(ANSI)的标准，在 1987 年成为国际标准化组织(ISO)的标准。

到 1986 年，ANSI 和 ISO 标准组正式采用了标准的“数据库语言 SQL”语言定义。该标准的新版本于 1989 年、1992 年、1996 年、1999 年、2003 年、2006 年、2008 年、2011 年、2016 年和 2019 年发布。

SQL 在几个方面偏离了它的理论基础，关系模型和元组演算。在那个模型中，**一个表是一组元组**，而 SQL 中的**，表和查询结果是行的列表**；同一行可能会出现多次，并且可以在查询中使用行的顺序(例如，在 LIMIT 子句中)。

# 对象-关系阻抗不匹配

对象-关系阻抗不匹配是当用面向对象编程语言或风格编写的应用程序服务于关系数据库管理系统(RDBMS)时经常遇到的一组概念和技术困难，特别是因为对象或类定义 ***必须映射到由关系模式定义的数据库表* s** 。

对象(实例)相互引用，因此形成一个图。相反，关系模式是**表格**并且基于关系代数，关系代数定义了链接的异构元组(将数据字段分组到一个“行”中，每个字段具有不同的类型)。

… *关系模型有一个内在的、相对较小的、定义良好的基本操作符集，用于数据的查询和操作，而面向对象语言通常通过定制的或低级的、特定于案例和物理访问路径的命令式操作来处理查询和操作。*一些面向对象语言确实支持声明性查询子语言，但是因为面向对象语言通常处理列表或者哈希表，操作原语必然不同于关系模型的基于集合的操作。
的
与**的并发**和**的事务**方面也有显著的不同。特别是，*事务，数据库执行的最小工作单元，在关系数据库中比任何由面向对象语言中的类执行的操作都要大得多。关系数据库中的事务是任意数据操作的动态有界集合，而面向对象语言中的事务粒度通常要细得多。*

在面向对象编程中，数据管理任务作用于几乎总是非标量值的对象。例如，考虑一个地址簿条目，它表示一个人以及零个或多个电话号码和零个或多个地址。这可以在一个面向对象的实现中用一个“Person object”来建模，这个“Person object”具有一个属性/字段来保存条目包含的每个数据项:人名、电话号码列表和地址列表。电话号码列表本身包含“电话号码对象”等等。编程语言将每个这样的地址簿条目视为单个对象(例如，它可以被包含对象指针的单个变量引用)。各种方法可以与该对象相关联，例如返回首选电话号码、家庭地址等的方法。

相比之下，直到最近，许多流行的数据库产品，如 SQL 数据库管理系统(DBMS)都不是面向对象的，只能存储和操作标量值，如表中组织的整数和字符串。程序员必须将对象值转换成简单的值组以存储在数据库中(并在检索时将它们转换回来)，或者在程序中只使用简单的标量值。

> 问题的核心是将对象的逻辑表示转换成原子形式，能够存储在数据库中，同时保留对象的属性及其关系，以便在需要时可以作为对象重新加载。如果实现了这种存储和检索功能，则称这些对象是持久的。

# 天真的解决方案

第一个 web 应用程序非常简单，没有分层——所有工作都在一层完成，包括连接到数据库、检索结果集以及将它们转换为对象。对于简单的 web 应用程序来说，这已经足够了。当 web 应用程序的复杂性开始增长时，就应用了**“分工”原则**。最初的分离是使用 3 层应用程序或模型-视图-控制器(MVC)设计模式。如今，MVC 被认为是过时的，但它的一些变体如 [MVP](https://ru.wikipedia.org/wiki/Model-View-Presenter) 或 [MVVM](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93viewmodel) 仍在使用。

## 数据访问层

最终，访问 RDBMS 的指定层成为任何 web 应用程序中的标准配置。

故事的其余部分将列出不同的实现技术及其演变。

但首先，我想重申什么是 DAL。DAL 是一个建筑术语。

数据访问层(DAL)是位于业务逻辑层和持久性/存储层之间的系统层。DAL 可以是一个单独的类，也可以由多个对象组成。这些对象实际上可以是任何东西，可以是 POJO(普通对象)、DAOs(数据访问对象——下面将详细介绍)、Repository(下面将详细介绍)。它可能是第三方对象关系映射工具(ORM，下面会详细介绍)，如 Spring Data、Hibernate、TopLink、JPA 或 Spring JDBC 模板或 SqlAlchemy(有或没有 ORM 功能)。**它是隐藏数据访问逻辑复杂性的一个门面。业务层与这个外观对话，从他的角度来看，他只处理对象。**

引入不同的层来处理数据访问逻辑为进一步的**专门化**打开了大门。首先，我们注意到同一个 DAL 可以用在不同的 web 应用程序中——我们对 web 应用程序有完全不同的业务需求，但是两者都有相似的数据模式存储在 RDBMS 中，所以我们可以重用同一个 DAL。

下一步是**外包**这一层。这可以通过两种截然不同的方式实现——作为独立的**库**或**框架**。库方法的例子是 Spring JDBC、Spring Data 或没有 ORM 的 SqlAlchemy。框架的例子将是 SqlAlchemy 与 ORM，JPA，实体框架核心(。NET)或任何其他 ORM 框架。

**库和框架有什么区别？**参见[框架 vs 库](https://alex-ber.medium.com/framework-vs-library-83931b6a168)

例如，Spring 是非介入式框架。**应用程序代码不需要直接依赖任何 Spring 对象。ORM 框架通常具有很强的侵入性。我确实有使用它们的经验，但是下面会有更多的介绍。另一个例子是 JPA。**

我们回达尔吧。正如我上面所说的，它的外包创造了两种不同的方式来使用它作为库(例如 Spring JDBC)或框架(例如 Spring Data 或带有 ORM 的 SqlAlchemy)。然而在 2010 年，出现了完全不同方法，以完全不同的方式解决这个问题。*这是 ORM 框架全线失败的最好证据*。这不是劳动的*、*的深化，而是**、*的深化，新知识的*、**、*。当然，我说的是 NoSQL。例如，MongoDB 是面向文档的数据库程序。作为一个 NoSQL 数据库程序，MongoDB 使用带有可选模式的类似 JSON 的文档。它创建于 2009 年，在 2013 年左右变得相当流行。**如果你在 MongoDB(或其他 NoSQL)中选择加入，你不需要做任何 ORM** ，你可以将你的实体直接存储到 MongoDB，只需要将它们转换成 JSON(就像你在控制器中与客户端通信一样)。所以，随着 ***的深化，新知识*** 我们可以**一起排除** ORM 步骤。不幸的是，这种方法有它自己的缺点，我将在下面讨论它们。因此，在过去几年中，我们已经开始看到一种新趋势，即 SQL 和 NoSQL 数据库之间的融合。更多详情请见下文。在故事的最后，我将描述我在当前从事的 Python 项目中的一些架构选择*

# 数据访问对象

DAO 是实现数据访问层的经典方法。截至 2021 年，这仍然是我提取数据持久性的首选方式。引用:

> *2。刀模*
> 
> *数据访问对象模式，又名 DAO 模式，* ***是数据持久性的抽象，被认为更接近底层存储，它通常以表为中心*** *。*
> 
> *因此，在许多情况下，我们的 Dao 匹配数据库表，允许以更直接的方式从存储中发送/检索数据，隐藏难看的查询。*
> 
> *让我们来看看 DAO 模式的一个简单实现。*
> 
> *2.1。用户*
> 
> 首先，让我们创建一个基本的用户域类:
> 
> `*public class User {
> private Long id;
> private String userName;
> private String firstName;
> private String email;*`
> 
> `*// getters and setters
> }*`
> 
> *2.2。用户道*
> 
> *然后，我们将创建 UserDao 接口，为用户域提供简单的 CRUD 操作:*
> 
> `*public interface UserDao {
> void create(User user);
> User read(Long id);
> void update(User user);
> void delete(String userName);
> }*`
> 
> *2.3。UserDaoImpl*
> 
> *最后，我们将创建实现 UserDao 接口的 UserDaoImpl 类:*
> 
> `*public class UserDaoImpl implements UserDao {
> private final EntityManager entityManager;
> 
> @Override
> public void create(User user) {
> entityManager.persist(user);
> }*`
> 
> `*@Override
> public User read(long id) {
> return entityManager.find(User.class, id);
> }*`
> 
> `*// ...
> }*`
> 
> *这里，为了简单起见，我们使用 JPA EntityManager 接口与底层存储进行交互，并为用户域提供数据访问机制。*

[https://www.baeldung.com/java-dao-vs-repository](https://www.baeldung.com/java-dao-vs-repository)

为了实现数据访问逻辑，DAO 可以使用任何库，比如 Spring JDBC 甚至 framework。

我见过许多通过 Dao 实现的 DAL 层。我甚至见过一个支持 MySQL，Postgress 和 Hive 的。它在 Java 1.6 上有非常好的设计。当 Lambda 和接口上的默认方法被添加时，我已经将其现代化为 Java 8——这是添加了*函数编程*的一些元素。因此，一些在 Java 1.6 上非常好的设计决策可能会被改变。在我最近的 Python 项目中，我使用 SQLAlchemy 通过许多 Dao 实现了 DAL 层，而没有 ORM。以下是更多相关信息。看起来大致是这样的:

第 131–152 行应该在某个 DAO 上组织成一个方法。这类似于上面 Java 代码中的`*public User read(long id)*`。

# 贮藏室ˌ仓库

Repository 也处理数据并隐藏类似于 DAO 的查询。然而，它位于更高的层次，更接近于应用程序的业务逻辑。

存储库是对象集合的抽象。

Repository 可以用 DAO 实现，但是你不能反过来做。

存储库将被认为更接近域，只处理聚合根。

> *来自埃文斯 DDD:*
> 
> 聚合是一组相关联的对象，出于数据更改的目的，我们将它们视为一个单元。每个集合都有一个根和一个边界。边界定义了集合内部的内容。根是聚合中包含的单个特定实体。
> 
> *和:*
> 
> 根是集合中唯一允许外部对象引用的成员。]
> 
> *这意味着聚合根是唯一可以从存储库中加载的对象。*
> 
> *一个例子是包含一个* `*Customer*` *实体和一个* `*Address*` *实体的模型。我们绝不会直接从模型中访问一个* `*Address*` *实体，因为没有关联* `*Customer*` *的上下文，它就没有意义。所以我们可以说* `*Customer*` *和* `*Address*` *一起形成一个集合，而* `*Customer*` *是一个集合根。*

[https://stackoverflow.com/a/1958765/1137529](https://stackoverflow.com/a/1958765/1137529)

存储库通常是一个更窄的接口。应该是简单的对象集合，有`Get(id)`、`Find(ISpecification)`、`Add(Entity)`。

像`Update`这样的方法适用于`DAO`，但不适用于`Repository`——当使用`Repository`时，对实体的更改通常会被单独的 UnitOfWork 跟踪。

示例:

[https://examples . javacodegeeks . com/enterprise-Java/spring/data/spring-data-jparepository-example/](https://examples.javacodegeeks.com/enterprise-java/spring/data/spring-data-jparepository-example/)

请在上面的链接中查看完整的示例细节。

另一个例子，引用:

> *…如何给 Spring 数据 JPA* `*CrudRepository*` *和 MongoDB* `*MongoRepository*`添加自定义方法
> 
> *1。CrudRepository*
> 
> *1.1 回顾一个* `*CustomerRepository*` *，我们将向这个资源库添加一个自定义方法。*

> *1.2 创建一个接口。*

> *1.3 对于实现类，类名非常严格，需要遵循“核心库接口+ Impl”模式。未能遵循此模式将导致“无法找到 Spring 属性”错误消息。*

> *1.4 更新* `*CustomerRepository*` *扩展新的* `*CustomerRepositoryCustomAbc*` *界面。*

> *搞定。*
> 
> *2。MongoRepository*
> 
> *2.1 另一个例子给* `*MongoRepository*`增加了一个新的“更新特定字段”方法

> *2.2 自定义界面。*

> *2.3 自定义实现。*

> *搞定。*

[https://mkyong . com/spring-data/spring-data-add-custom-method-to-repository/](https://mkyong.com/spring-data/spring-data-add-custom-method-to-repository/)

请参见[https://docs . spring . io/spring-data/JPA/docs/2.6 . x/reference/html/# repositories . create-instances](https://docs.spring.io/spring-data/jpa/docs/2.6.x/reference/html/#repositories.create-instances)

[https://docs . spring . io/spring-data/JPA/docs/2.6 . x/reference/html/# repositories . custom-implementations](https://docs.spring.io/spring-data/jpa/docs/2.6.x/reference/html/#repositories.custom-implementations)

## 仓库是 SQL DML 的再造

我想在上面的第 19 行扩展一下。

`Query query = new Query(Criteria.where(“domain”).is(domain));`

SQL DML 是**数据操作语言**的简称，它处理数据操作，包括最常见的 SQL 语句，如 SELECT、INSERT、UPDATE、DELETE 等。，它用于存储、修改、检索、删除和更新数据库中的数据。

> 使用存储库设计模式基本上是以面向对象的方式重新发明数据操作语言。

我想从我上面提供的同一链接中再举一个例子来说明我的观点，来自
[https://blog . Marc Nuri . com/spring-data-MongoDB-custom-repository-implementation](https://blog.marcnuri.com/spring-data-mongodb-custom-repository-implementation)同样，你可以通过链接查看所有细节:

尽管上面提供的所有例子都是用 Java(用 Spring)编写的，但这些例子并不是 Java 特有的。下面是 Python 中相同的存储库设计模式(使用 SqlAlchemy):

[https://docs . sqlalchemy . org/en/14/core/tutorial . html #连接词](https://docs.sqlalchemy.org/en/14/core/tutorial.html#conjunctions)

您在 Python 和 Java 中都使用了面向对象的包装代码，这些代码将在 library\framework 中内部转换为普通 SQL。在最后一个例子中，这样的伪 SQL 打印在第 12–15 行。

## 个人对存储库的看法

我已经在许多项目中成功地使用了存储库模式。但是我用了一种不太标准的方式。我的按钮行是:

> *仓库设计模式适合:*
> 
> *1。简单的 CRUD 应用。*
> 
> *2。如果你有少量的实体。*
> 
> *3。如果您通常需要处理(选择、更新等)少量的实体。*
> 
> *否则，你必须把 SQL DML 带到你的应用程序中，这在我看来是错误的*。

我在实践中所做的是将仓库作为 DAO 来使用。所以，每个实体都是聚合根，我确实写了一些**数据操作代码**把不同的实体粘在一起。如果您的应用程序满足以上几点，那么这样做是可以的。否则，编写这种粘合代码的复杂性要比使用 DAO 设计模式高得多。

如果您的应用程序满足以上几点，您就可以安全地使用 DAO。您确实会编写一些样板代码(特别是对于简单的 CRUD 应用程序),您可以使用存储库设计模式来避免这些代码(我怀疑这是最初引入存储库设计模式的主要动机),这些样板代码将分散在许多 DAO 类中，但是与以面向对象的方式掌握 SQL DML 相比，这是很低的代价。我将在下面的下一节详细阐述这一点。

正如我上面所说的，我更喜欢使用 DAO 设计模式，但是我对使用存储库设计模式没有问题。

# TopLink、Hibernate 和 JPA

在 2013 年发布 JPA 2.1 规范之前，我从未听说过它，也没有在实践中看到有人在使用 JPA。

2013 年后，我使用 Hibernate 作为 JPA 规范的实现。

但是我以前确实听说过冬眠。我从理论上了解过，也和一些有实践经验的人谈过。回顾过去，我发现他们是对的。

Hibernate 的一个主要卖点是

> 编写数据操作代码不需要了解 SQL。

此外，Hibernate 的建议是用一些额外的 Hibernate 元数据将实体建模为对象(那时，它是在 XML 文件中，后来被 Java 注释所取代),然后**让 Hibernate 从实体中生成 DB 表。**

实际上，据我所知，这在实践中从未奏效。Hibernate 创建的 DB 模式不是最佳的。查询/插入/更新这样的数据库模式非常慢。

实际上，我已经在 2014 年尝试用 JPA 做这件事(在 2020 年尝试用 SQLAlchemy 做这件事),并且我已经回顾了 DB 模式。例如，在这两种情况下，都没有创建一些必要的索引。在 SqlAlchemy 案例中，我很好地控制了从对象属性到 DB 表列的映射。在 JPA **案例中，如果不彻底阅读整个 JPA 规范**(是的，我被迫全部阅读),一些常见的 DB 映射很难用它建模

> *使用 Hibernate 映射文件时，对于索引集合的双向映射(其中一端表示为* `*<list>*` *或* `*<map>*` *)还有一些额外的注意事项。如果有一个子类的属性映射到索引列，您可以在集合映射上使用* `*inverse="true"*` *..*
> 
> *有三种可能的方法来映射三元关联。一种方法是使用带有关联的* `*Map*` *作为其索引…*
> 
> 第二种方法是将关联改造成一个实体类。这是最常见的方法。最后一种选择是使用复合元素，这将在后面讨论。

[https://docs . JBoss . org/hibernate/ORM/4.1/manual/en-US/html/ch07 . html # collections-advanced mappings](https://docs.jboss.org/hibernate/orm/4.1/manual/en-US/html/ch07.html#collections-advancedmappings)

另请参见[https://hibernate.atlassian.net/browse/HHH-8229](https://hibernate.atlassian.net/browse/HHH-8229)和[https://stack overflow . com/questions/40379807/what-is-the-different-between-many toone-optional-flag-vs-join column-nullable-f](https://stackoverflow.com/questions/40379807/what-is-the-different-between-manytoone-optional-flag-vs-joincolumn-nullable-f)了解为什么如何使用 Hibernate 进行属性映射“并不明显”的明确示例。

让我们回到简单的冬眠。主要困难如下:

1.  *很难判断什么属性映射是正确的。*最终，唯一可靠的方法是查看生成的 SQL。但是如果我被迫使用 SQL，那么这一层是不必要的。
2.  *缓存有多种级别。*另请参见[https://Alex-ber . medium . com/hibernate-recursive-initial ization-d 738 CD 01035](https://alex-ber.medium.com/hibernate-recursive-initialization-d738cd01035)当您更改 entitles \ DB 表时，可能会遇到许多恼人的错误。您可以在缓存中的某个地方保存旧版本实体的旧副本，它将只在某个执行路径上启动。由于 Hibernate 的优化，重新运行整个应用程序可能是不够的。您可能还需要清理和重建所有的类。这很烦人。JPA 也会出现这种情况。类似的事情也发生在 ORM 的 SqlAlchemy 上。它也有所有实体的缓存。无论如何，重新运行 Python 应用程序都会完成工作(我从来不需要手动删除*。pyc 文件)。
3.  *从实体定义创建数据库模式存在严重的性能问题。*对于 Hibernate、JPA、带 ORM 的 SQLAlchemy 和不带 ORM 的 SQLAlchemy 都是如此。在没有 ORM 的 SQLAlchemy 的情况下，理论上可以创建最佳的 DB 模式(使用定制的编译器插件)，但是工作量相当大。开箱即用，这是次优的。

# 作业的装配区（JobPackArea）

JPA 1.0 是由 Hibernate 和 TopLink 合并而成的，大约 2/3 来自 Hibernate，1/3 来自 TopLink。在 JPA 1.0 规范之后，两种产品都发布了默认情况下符合 JPA 规范的新版本。您可以选择将特定于 Hibernate(或特定于 TopLink)特性与 JPA 一起使用，或者继续直接使用 Hibernate(或 TopLink ),但是据我所知，大多数项目最终都切换到了 JPA。我参与过许多项目，这些项目都将 Hibernate 作为 JPA 规范的实现。

正如我在 2014 年说过的，我与 JPA 合作过。为了能够做到这一点，我被迫阅读了整个 JPA 规范。我选择了另一种模式—

> 我已经编写了 SQL DDL 来创建 DB 模式，然后我使用了注释来映射表的列的实体属性。

所以，我没有跳过设计数据库模式这一步，而是用传统的方法，通过规范化、索引等来完成。

使用这种方法，我没有遇到性能问题，至少对于少量到中等数量的数据。

我已经在 JPA 2.1 和没有 ORM 的 SQLAlchemy 中成功地使用了这种方法。这个很好用。

***注:*** 我想强调几件事:

1.  虽然大多数示例来自 Java(也有一些来自 Python)，但在。Net 和任何其他编程语言。
2.  JPA 在历史上是由 Sun 作为 Java EE 的一部分开发的。JPA 是 Java 持久性 API。Hibernate 被*事实上的*引用实现。在 Oracle 收购 Sun 之后，**由于 JPA 的复杂性，Oracle 将它外包出去**，现在它被称为 Jakarta Persistence。JPA 的参考实现是 EclipseLink。

最终，我认为增加这一层没什么好处。对我来说，跳过它并使用 DAO 来处理对象关系阻抗不匹配要容易得多。

# **ORDBM** S **:对象-关系数据库管理系统**

ORDBMS 或 order(对象-关系数据库)是一个类似于关系数据库的数据库管理系统(DBMS ),但它具有面向对象的数据库模型:数据库模式和查询语言直接支持对象、类和继承。此外，就像纯关系系统一样，它支持使用**定制数据类型**(稍后将详细介绍)和方法来扩展数据模型。

对象-关系数据库可以说是在关系数据库和面向对象数据库之间提供了一个中间地带。在对象关系数据库中，这种方法本质上是关系数据库的方法:数据驻留在数据库中，由查询语言中的查询共同操作；另一个极端是 OODBMSes，其中数据库本质上是用面向对象编程语言编写的软件的持久对象存储，具有用于存储和检索对象的编程 API，很少或没有对查询的特定支持。

对象-关系数据库的基本需求源于这样一个事实，即关系数据库和对象数据库各有优缺点。关系数据库系统与数学关系的同构允许它利用集合论中的许多有用的技术和定理。但是这些类型的数据库对于某些类型的应用程序来说并不是最佳的。面向对象的数据库模型允许像集合和列表、任意用户定义的数据类型以及嵌套对象这样的容器。这带来了应用类型系统和数据库类型系统之间的通用性，消除了阻抗不匹配的任何问题。但是，与关系数据库不同，对象数据库不为它们的深入分析提供任何数学基础。

对象关系数据库管理系统产生于 20 世纪 90 年代早期的研究。该研究通过添加对象概念扩展了现有的关系数据库概念。研究人员旨在保留一种基于 ***关系代数*** 的声明式查询语言，作为该架构的核心组件。可能是最著名的研究项目，Postgres(加州大学伯克利分校)，产生了两个追溯其血统的研究产品:Illustra 和 **PostgreSQL** 。到了下一个十年，PostgreSQL 已经成为一个商业上可行的数据库，并且是当前几个保持其 ORDBMS 特性的产品的基础。

早期的对象-关系数据库的许多想法已经通过**结构化类型**很大程度上融入到了 **SQL-1999** 中。事实上，任何符合 SQL-1999 面向对象方面的产品都可以被描述为对象关系数据库管理产品。*下面我们来看看 SQL-1999。*

# NoSQL

显然，我不是唯一一个认为用 ORM 框架来解决对象关系阻抗不匹配的人。最起码最后调试费时费力。为了掌握它，你只需要学习新的工具——Hibernate、JPA、SQLAlchemy 等，但是你也应该理解底层生成的 SQL 和*缓存*。

传统的基于 SQL 的 RDBMS 的另一个问题是**数据爆炸—** 可用数据集的大小和数量迅速增长。更多信息请见 [NoSQL，MongoDB，HiveQL](https://alex-ber.medium.com/nosql-mongodb-hiveql-b52c80ddc90a)

# SQL:2016

SQL:2016 引入了 44 个新的可选特性。其中 22 个属于 **JSON** 功能，另外 10 个与多态表函数相关。该标准的新增内容包括:

*   JSON:创建 JSON 文档、访问 JSON 文档的各个部分以及检查字符串是否包含有效的 JSON 数据的函数
*   行模式识别:根据正则表达式模式匹配一系列行
*   日期和时间格式和解析
*   LISTAGG:将一组行中的值转换成分隔字符串的函数
*   多态表函数:没有预定义返回类型的表函数
*   新数据类型 DECFLOAT

# SQL:1999

SQL:1999 标准要求布尔类型，但是许多商业 SQL 服务器(Oracle 数据库、IBM DB2)不支持它作为列类型、变量类型，也不允许它出现在结果集中。每 1–8 位字段占用磁盘上一个完整字节的空间。MySQL 将“BOOLEAN”解释为 TINYINT (8 位有符号整数)的同义词。PostgreSQL 提供了一个符合标准的布尔类型。

# 结构化用户定义类型

这些是 SQL:1999 中对象-关系数据库扩展的主干。它们类似于面向对象编程语言中的类。SQL:1999 只允许单一继承。

SQL:1999 标准在 SQL 中引入了许多对象关系数据库特性，其中主要是结构化用户定义类型，通常称为结构化类型。这些既可以用带创建类型的普通 SQL 定义，也可以通过 SQL/JRT 用 Java 定义。SQL 结构化类型允许单一继承。

Oracle 数据库、IBM DB2、PostgreSQL 和 Microsoft SQL Server 都不同程度地支持结构化类型，尽管后者只允许 CLR 中定义的结构化类型。

# SQL 示例
对象结构化类型

为了使用 Oracle 数据库定义自定义结构类型，可以使用如下语句:

`CREATE TYPE Person_Type AS OBJECT (
person_title VARCHAR2(10),
person_first_name VARCHAR2(20),
person_last_name VARCHAR2(20),
)
NOT FINAL;`

然后，可以使用这种结构类型创建一个表，该表还包含 Person_Type 中定义的所有列:

`CREATE TABLE Person_Table OF Person_Type;`
自定义结构类型支持继承，这意味着用户可以创建另一个继承自上一个类型的类型。然而，NOT FINAL 语句必须包含在基本结构类型定义中，以便允许创建任何其他子类型。

`CREATE TYPE Student_Type UNDER Person_Type (
matriculation_number NUMBER(10)
);` 然后可以使用 Student_Type 来创建一个 Student_Table，该表还将包含 Person_Type 中定义的所有列。主键和约束应该在创建表期间或之后定义，不能在结构类型本身内部定义。

`CREATE TABLE Student_Table OF Student_Type (
matriculation_number PRIMARY KEY,
CONSTRAINT person_title_not_null_constraint NOT NULL (person_title),
);` 为了支持更复杂的结构，每个自定义结构类型还可以包含其他类型:

```
CREATE TYPE Address_Type AS OBJECT (
 address_street VARCHAR2(30),
 address_city VARCHAR2(30),
);CREATE TYPE University AS OBJECT (
 university_name VARCHAR2(30),
 university_address Address_Type
);
```

# NoSQL 之后的 SQL

> 当前的 ISO SQL 标准没有提到关系模型，也没有使用关系术语或概念。SQL 在几个地方偏离了关系模型。

请注意，很少有数据库服务器实现完整的 SQL 标准，尤其是不允许这些偏差。例如，尽管 NULL 无处不在，但允许表或匿名列中出现重复的列名并不常见。

> *** 重复行。*同一行可以在一个 SQL 表中出现多次。同一元组在一个关系中不能出现多次。* *匿名专栏。*SQL 表中的列可以不命名，因此不能在表达式中引用。关系模型要求每个属性都被命名和引用。
> ** 重复的列名。*同一个 SQL 表中的两个或多个列可以有相同的名称，由于明显的模糊性，因此不能被引用。关系模型要求每个属性都是可引用的。
> ** 列顺序意义。*SQL 表中列的顺序是定义好的，也是重要的，一个结果是 SQL 的笛卡尔积和并的实现都是不可交换的。关系模型要求关系属性的任何排序都没有意义。* *视图没有勾选选项。*可以接受对未定义检查选项的视图的更新，但对数据库的结果更新不一定对其目标有明确的影响。例如，可以接受对 INSERT 的调用，但插入的行可能不会全部出现在视图中，或者对 UPDATE 的调用可能会导致行从视图中消失。关系模型要求对视图的更新具有与视图是基本 relvar 相同的效果。
> ** 无列表格无法识别。 *SQL 要求每个表至少有一列，但是有两个零度关系(基数 1 和 0 ),需要它们来表示不包含自由变量的谓词的扩展。* * NULL。这个特殊标记可以代替值出现在 SQL 中任何可以出现值的地方，特别是代替某行中的列值。与关系模型的偏离源于这样一个事实，即在 SQL 中实现这个特定概念涉及到三值逻辑、*的使用，在这种情况下，NULL 与其自身的比较不会产生 true，而是产生第三个真值 unknown 类似地，NULL 与自身以外的东西的比较不会产生 false，而是产生 unknown。正是因为比较中的这种行为，NULL 被描述为一个标记而不是一个值……*

SQL 标准的数组部分 **SQL-1999:增加了非标量类型(数组)**

SQL 标准的 JSON 部分 [**SQL:2016**](https://en.wikipedia.org/wiki/SQL:2016) **:增加了 JSON** :创建 JSON 文档、访问 JSON 文档部分、检查字符串是否包含有效 JSON 数据的函数。所有这些都是可选功能。

[https://www . citus data . com/blog/2016/07/14/choosing-no SQL-h store-json-jsonb/](https://www.citusdata.com/blog/2016/07/14/choosing-nosql-hstore-json-jsonb/)
[https://Clark Dave . net/2013/06/what-can-you-do-with-PostgreSQL-and-JSON/](https://clarkdave.net/2013/06/what-can-you-do-with-postgresql-and-json/)
[https://www.postgresql.org/docs/release/9.4.0/](https://www.postgresql.org/docs/release/9.4.0/)
[https://en.wikipedia.org/wiki/PostgreSQL](https://en.wikipedia.org/wiki/PostgreSQL)(搜索 JSON)

2016 年 1 月， [PostgreSQL 9.5 版本](https://en.wikipedia.org/wiki/PostgreSQL)是第一个提供高效 JSON 内部数据类型(JSONB)的 FOSS OODBMS，它具有完整的函数和操作集，可用于所有基本的关系和非关系操作。[https://en.wikipedia.org/wiki/Object_database](https://en.wikipedia.org/wiki/Object_database)

*   2006 XML 作为 Oracle 中的 CLOB
*   2012 JSON as CLOB in Postgress

# NoSQL 和 SQL 的融合

如您所见，在与 SQL 和 NoSQL 产生巨大分歧，有时在两个不同的分支中并行开发之后，2012 年，postgresse 中增加了对 JSON 的一些初始支持(大约同时还有另一个 RDBMS)，然后 JSON 支持被添加到 SQL-2016 标准中，并在 postgresse 和任何其他 RDBMS 中重新实现。

另一方面，在最新版本中增加了许多 NoSQL 缺乏的功能，典型的例子是 HIVEQL。当 HIVE 启动时，它被故意选择为不符合 SQL-92，但是实践促使他们最终实现它。

这是一次非常漫长的旅程，我已经将其中一部分摘录为独立的故事，所以你可以选择跳过它们。现在，我想给你你读过的鸟:

*   传统的 SQL 需要一些到 Python/Java 中使用的对象的映射。最好在 DAL 中完成(以便在其他应用程序中重用)。
*   DAL 层可以实现为 DAO 或 Repository。我个人的选择是刀。
*   DAL 层也可以有选择地实现为 ORM，但我宁愿不使用它。
*   你可以一起使用 NoSQL 数据库，例如 HIVEQL 或 MongoDB。你的应用代码不要管，用 DAL 做门面就行了。
*   您可以将 Postgress 用作 SQL，但是仍然使用一些对象类型，比如 arrays 和 JSON，这完全没问题。不属于关系模型是“违背”早期 SQL 标准精神的，但最新的 SQL 标准取消了对关系模型的任何提及。
*   试图用 ORM 作为*分工*已经失败(IMHO)。***【NoSQL】***深化新知，一方面通过废除关系模型回归基础，通过编写一些易于维护的手动代码进入基础道层(存储库也可以)就是答案。