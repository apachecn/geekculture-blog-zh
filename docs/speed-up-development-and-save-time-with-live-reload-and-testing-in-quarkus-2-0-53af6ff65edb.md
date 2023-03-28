# 通过 Quarkus 2.0 中的实时重新加载和测试，加速开发并节省时间

> 原文：<https://medium.com/geekculture/speed-up-development-and-save-time-with-live-reload-and-testing-in-quarkus-2-0-53af6ff65edb?source=collection_archive---------16----------------------->

![](img/e24bc43d54c8254d53e91776779abb09.png)

Quarkus 附带了一个**工具链**，使开发人员能够通过它对所有主要 ide 的扩展快速开始他们的开发。它的一个突出且易于使用的特性是它能够在**开发模式**下执行**实时重新加载**，让开发人员无需重启即可动态更改实现。有了新的**版本 2.0** ，开发者可以做更多。现在在开发模式中有一个**交互式 shell** ，它提供了一组不同的选项来更精细地控制在实时模式中运行什么。 **Quarkus** 还附带了一个 **CLI** 二进制文件，支持进一步快速开发和项目设置。

![](img/ce7ac1b0824d01cdc66b46c24cdc4c14.png)

Quarkus 2.0 刚刚看到了曙光，并在不同的领域提出了很多改进。

[](https://github.com/quarkusio/quarkus/releases/tag/2.0.0.Final) [## 最终版 quarkusio/quarkus

### Quarkus:超音速亚原子 Java。。在 GitHub 上创建一个帐户，为 quarkusio/quarkus 的发展做出贡献。

github.com](https://github.com/quarkusio/quarkus/releases/tag/2.0.0.Final) 

# 实时重新加载(开发模式)

Quarkus 之前提供了一种'[开发模式](https://access.redhat.com/documentation/en-us/red_hat_build_of_quarkus/1.3/html/developing_and_compiling_your_quarkus_applications_with_apache_maven/proc-quarkus-dev-mode_quarkus-maven)'，这是一种热重新部署机制，通过这种机制，当在浏览器中刷新时，应用程序中的代码更改将被重新编译和重新加载。除了在本地广泛使用之外，还可以在远程容器上使用，提供远程开发模式。

```
$ ./mvnw quarkus:dev
```

随着**版本 2.0** 如上所述，我们提供了几个新的选项。当按下`[r]`时，测试在当前开发加载的类的套件中运行。

```
All 1 tests are passing (0 skipped), 1 tests were run in 1547ms. Tests completed at 22:05:53.
Press [r] to re-run, [v] to view full results, [p] to pause, [h] for more options>
```

按下`[h]`,开发人员可以选择重新运行测试失败的套件，或者从之前的失败中继续调试(如果有的话)。

```
The following commands are available:
[r] — Re-run all tests
[f] — Re-run failed tests
[b] — Toggle ‘broken only’ mode, where only failing tests are run (disabled)
[v] — Print failures from the last test run
[o] — Toggle test output (disabled)
[p] — Pause tests
[i] — Toggle instrumentation based reload (disabled)
[l] — Toggle live reload (enabled)
[s] — Force live reload scan
[h] — Display this help
[q] — Quit
```

如果我们正在构建一个依赖于数据库的应用程序，人们可能会考虑以编程方式为测试建立一个数据库。然而，有一种无缝的方法可以让 Quarkus 通过应用所谓的零配置设置为您提供一个数据库层。

# 零配置设置(开发服务)

当在开发模式下测试或运行时，Quarkus 可以为您提供一个开箱即用的零配置数据库，这个特性被称为 DevServices。根据数据库类型，您可能需要安装 docker 才能使用该功能。以下开源数据库支持 DevServices:

*   Postgresql(容器)
*   MySQL(容器)
*   MariaDB(容器)
*   H2(进行中)
*   Apache Derby(进行中)
*   DB2(容器)(需要许可接受)
*   MSSQL(容器)(需要许可接受)

如果你想使用 DevServices，那么你需要做的就是包含你想要的数据库类型的相关扩展(反应式或 JDBC，或两者都有)，不要配置数据库 URL、用户名和密码，Quarkus 将提供数据库，你可以开始编码而不用担心配置。通过这种方式，您可以运行一个本地数据库，并且不需要弄乱它，反之亦然，这为开发人员提供了测试隔离。这实际上使用 TestContainer 作为一个框架，允许开发人员快速测试他们的应用程序，而不必过多关注数据库层的设置和业务逻辑。

# 夸尔库斯 CLI

在 2.0 版 Quarkus CLI 中，您可以使用底层项目的构建工具创建项目、管理扩展以及执行基本的构建和开发命令。

```
curl -Ls https://sh.jbang.dev | bash -s - app install --fresh --force quarkus@quarkusio
```

[](https://quarkus.io/version/main/guides/cli-tooling) [## 使用 Quarkus 命令行界面(CLI)构建 Quarkus 应用

### 要创建一个新项目，我们使用 create-project 命令:这将创建一个名为“hello-world”的文件夹在您的…

quarkus.io](https://quarkus.io/version/main/guides/cli-tooling) 

使用 CLI，我们可以执行以下命令。

```
$ quarkus --version
Client Version {quarkus-version}
```

# 创建 Quarkus 项目

```
$ quarkus create
-----------

applying codestarts...
📚  java
🔨  maven
📦  quarkus
📝  config-properties
🔧  dockerfiles
🔧  maven-wrapper
🚀  resteasy-codestart

-----------
[SUCCESS] ✅ quarkus project has been successfully generated in:
--> /<output-dir>/code-with-quarkus
```

我们也可以应用相关的工件和项目配置。

```
$ quarkus create app --group-id com.foo --artifact-id bar --version 1.0
version 1.0
-----------

applying codestarts...
📚  java
🔨  maven
📦  quarkus
📝  config-properties
🔧  dockerfiles
🔧  maven-wrapper
🚀  resteasy-codestart

-----------
[SUCCESS] ✅ quarkus project has been successfully generated in:
--> /<output-dir>/bar
-----------
```

从这里开始，我们可以根据应用程序的性质来添加和删除扩展。首先列出我们已经有的。

# 列出扩展名

列出项目中已安装的扩展。

```
$ quarkus ext ls
```

# 搜索扩展

使用`--installable`或`-i`选项列出可以从项目使用的 Quarkus 平台安装的扩展。

使用搜索(`--search`或`-s`)可以缩小或过滤搜索范围。

```
$ quarkus ext list --concise -i -s jdbcJDBC Driver - DB2                                  quarkus-jdbc-db2
JDBC Driver - PostgreSQL                           quarkus-jdbc-postgresql
JDBC Driver - H2                                   quarkus-jdbc-h2
JDBC Driver - MariaDB                              quarkus-jdbc-mariadb
JDBC Driver - Microsoft SQL Server                 quarkus-jdbc-mssql
JDBC Driver - MySQL                                quarkus-jdbc-mysql
JDBC Driver - Oracle                               quarkus-jdbc-oracle
JDBC Driver - Derby                                quarkus-jdbc-derby
Elytron Security JDBC                              quarkus-elytron-security-jdbc
Agroal - Database connection pool                  quarkus-agroal
```

# 添加扩展

Quarkus CLI 可以使用' *add* '命令为您的项目添加一个或多个 Quarkus 扩展:

```
$ quarkus ext add kubernetes health
[SUCCESS] ✅ Extension io.quarkus:quarkus-kubernetes has been installed
[SUCCESS] ✅ Extension io.quarkus:quarkus-smallrye-health has been installed
```

# 删除扩展

Quarkus CLI 可以使用' *remove* '命令从您的项目中删除一个或多个扩展:

```
$ quarkus ext rm kubernetes
[SUCCESS] ✅ Extension io.quarkus:quarkus-kubernetes has been uninstalled
```

# 构建您的项目

要使用 Quarkus CLI 构建您的项目(在本例中使用默认配置):

```
$ quarkus build
```

# 使用 CLI 进入开发模式

```
$ quarkus dev --help
```

要从 Quarkus CLI 启动开发模式，请执行以下操作:

```
$ quarkus dev         

[INFO] Scanning for projects...
[INFO]
[INFO] ---------------------< org.acme:code-with-quarkus >---------------------
[INFO] Building code-with-quarkus 1.0.0-SNAPSHOT
[INFO] --------------------------------[ jar ]---------------------------------
[INFO]
...
Listening for transport dt_socket at address: 5005
__  ____  __  _____   ___  __ ____  ______
--/ __ \/ / / / _ | / _ \/ //_/ / / / __/
-/ /_/ / /_/ / __ |/ , _/ ,< / /_/ /\ \
--\___\_\____/_/ |_/_/|_/_/|_|\____/___/
2021-05-27 10:15:56,032 INFO  [io.quarkus] (Quarkus Main Thread) code-with-quarkus 1.0.0-SNAPSHOT on JVM (powered by Quarkus 999-SNAPSHOT) started in 1.387s. Listening on: http://localhost:8080
2021-05-27 10:15:56,035 INFO  [io.quarkus] (Quarkus Main Thread) Profile dev activated. Live Coding activated.
2021-05-27 10:15:56,035 INFO  [io.quarkus] (Quarkus Main Thread) Installed features: [cdi, resteasy, smallrye-context-propagation]--
Tests paused, press [r] to resume
```

有了这些构建工具和特性，开发人员可以更专注于应用程序方面的事情，而无需设置项目框架或数据库的繁琐过程。如果你更喜欢在线向导的方式，他们可以参考这个链接，在这里项目可以被引导到你的 GitHub 账户。

 [## Quarkus -从 code.quarkus.io 开始编码

### 编辑描述

code.quarkus.io](https://code.quarkus.io/) 

祝你好运！