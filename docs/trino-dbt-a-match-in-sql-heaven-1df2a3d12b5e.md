# trino+dbt = SQL 天堂里的匹配？

> 原文：<https://medium.com/geekculture/trino-dbt-a-match-in-sql-heaven-1df2a3d12b5e?source=collection_archive---------8----------------------->

![](img/8095d42430665e88f0f529395d654183.png)

如果您可以利用两个最成功的开源数据项目的力量，仅通过 SQL 查询来混合和加载来自不同来源(内部或云)的数据，会怎么样？

为什么不使用一流的 dbt 和 Trino 解决方案作为强大的 ETL 工具呢？

*   [**dbt**](https://www.getdbt.com/) (数据构建工具)使分析工程师能够通过简单地编写 SQL select 语句来转换他们仓库中的数据。dbt 负责将这些 SQL select 语句转换成表和视图([更多细节](https://docs.getdbt.com/docs/introduction))。
*   [**Trino**](https://trino.io/) ，前身为 [PrestoSQL](https://trino.io/blog/2020/12/27/announcing-trino.html) ，是一款用于大数据分析的快速分布式 SQL 查询引擎，可帮助您探索您的数据世界并混合多个数据源([更多详细信息](https://trino.io/docs/current/overview/use-cases.html))。

Trino 通过 1 个连接连接到多个不同的数据源([可用连接器](https://trino.io/docs/current/connector.html))，并大规模快速处理 SQL 查询，而 dbt 则处理这些 SQL 转换查询以创建表或视图。

完美契合，不是吗？会不会是 SQL 天堂里的天作之合？

让我们通过一个演示来看看我们可以用 dbt 和 Trino 做什么。通过这个简单的项目示例，您将能够:

*   启动 Trino 服务器。
*   从 Trino 连接到 Google BigQuery 数据集和内部 PostgreSQL 数据库。
*   通过 dbt 项目，连接 BigQuery 表和 PostgreSQL 表，并将结果写入本地 PostgreSQL 表。

代码和解释可在 [GitHub](https://github.com/victorcouste/trino-dbt-demo) 上获得。

# 要求

安装:

*   **Trino** — [安装说明](https://trino.io/docs/current/installation/deployment.html)，可以进行单机测试和演示。
*   **dbt** — [安装说明](https://docs.getdbt.com/dbt-cli/installation)
*   **dbt-presto** — [安装说明](https://docs.getdbt.com/reference/warehouse-profiles/presto-profile#installation-and-distribution)，这是 Presto dbt Python 插件。08/09/21 更新:新的 [**Trino dbt 插件**](https://docs.getdbt.com/reference/warehouse-profiles/trino-profile) 现已推出，使用起来会更好( [**安装、配置和源代码**](https://github.com/starburstdata/dbt-trino) )

# 设置

# 特里诺

如这里的[所述](https://trino.io/docs/current/installation/deployment.html#configuring-trino)，在启动 Trino 服务器之前，您需要:

*   在 **/etc** 文件夹中设置您的配置和属性文件。
*   用数据源连接定义您的[目录](https://trino.io/docs/current/installation/deployment.html#catalog-properties)。

对于这个演示项目，您可以使用在 [trino_etc](https://github.com/victorcouste/trino-dbt-demo/blob/main/trino_etc) github 文件夹中找到的配置、属性和目录文件。只需将这个 **/trino_etc** 文件夹的内容复制到您的 trino 服务器安装下的一个 **/etc** 文件夹中即可。

## 配置:

由于 [PrestoSQL 到 Trino 的重命名](https://trino.io/blog/2020/12/27/announcing-trino.html)，您需要强制将协议头命名为 Presto，以便 dbt-presto 插件能够很好地与最新的 Trino 版本一起工作。为此，需要在 Trino 配置文件[config . properties](https://github.com/victorcouste/trino-dbt-demo/blob/main/trino_etc/config.properties)([文档](https://trino.io/docs/current/admin/properties-general.html?highlight=alternate%20header#protocol-v1-alternate-header-name))中添加额外的`protocol.v1.alternate-header-name=Presto`属性。

```
coordinator=true
node-scheduler.include-coordinator=true
http-server.http.port=8080
query.max-memory=5GB
query.max-memory-per-node=1GB
query.max-total-memory-per-node=2GB
discovery-server.enabled=true
discovery.uri=http://localhost:8080
protocol.v1.alternate-header-name=Presto
```

08/09/21 更新:使用新的 [Trino dbt 插件](https://docs.getdbt.com/reference/warehouse-profiles/trino-profile) ( [GitHu](https://github.com/starburstdata/dbt-trino) b)，您不再需要更改`protocol.v1.alternate-header-name`属性。

## 目录:

您需要定义想要通过 Trino 连接的数据源。

在本演示中，我们将连接到一个 Google BigQuery 数据集和一个内部 PostgreSQL 数据库。

**PostgreSQL**

必须将 [postgres.properties](https://github.com/victorcouste/trino-dbt-demo/blob/main/trino_etc/catalog/postgresql.properties) 文件复制到您的 **etc/catalog** Trino 文件夹中。您需要设置 PostgreSQL 数据库的主机名、数据库和凭证( [Trino doc](https://trino.io/docs/current/connector/postgresql.html) )，如下所示:

```
connector.name=postgresql
connection-url=jdbc:postgresql://example.net:5432/database
connection-user=root
connection-password=secret
allow-drop-table=true
```

注意，您需要添加`allow-drop-table=true`以便 dbt 可以通过 Trino 删除表。

**BigQuery**

对于 BigQuery，我们将连接到 dbt 公共项目 **dbt-tutorial** 。必须将 [bigquery.properties](https://github.com/victorcouste/trino-dbt-demo/blob/main/trino_etc/catalog/bigquery.properties) 文件复制到您的 **etc/catalog** Trino 文件夹中，并且您需要使用您的 Google Cloud 项目密钥设置**big query . credentials-file**或**big query . credentials-key**([Trino doc](https://trino.io/docs/current/connector/bigquery.html))。

要获得这个项目的 json 密钥文件，你可以阅读 [Google 文档](https://cloud.google.com/docs/authentication/getting-started)或 [dbt 文档](https://docs.getdbt.com/tutorial/setting-up#create-a-bigquery-project)中关于 BigQuery 项目和 JSON 密钥文件创建的解释。

```
connector.name=bigquery
bigquery.project-id=dbt-tutorial
bigquery.credentials-file=/your_folder/google-serviceaccount.json
```

# dbt

## 个人资料:

您需要将 [trino_profile.yml](https://github.com/victorcouste/trino-dbt-demo/blob/main/trino_profile.yml) 文件复制为您家中的 profiles.yml。dbt 文件夹 **~/。dbt/profiles.yml** 。

关于 [dbt 轮廓](https://docs.getdbt.com/dbt-cli/configure-your-profile)文件和 [Presto/Trino 轮廓](https://docs.getdbt.com/reference/warehouse-profiles/presto-profile)的文件。

默认目录是 PostgreSQL，因为我们将在 PostgreSQL 中编写。

```
trino:
  target: dev
  outputs:
    dev:
      type: presto
      method: none  # optional, one of {none | ldap | kerberos}
      user: admin
      password:  # required if method is ldap or kerberos
      catalog: postgresql
      host: localhost
      port: 8080
      schema: public
      threads: 1
```

08/09/21 更新:使用新的 [Trino dbt 插件](https://docs.getdbt.com/reference/warehouse-profiles/trino-profile) ( [GitHu](https://github.com/starburstdata/dbt-trino) b)，概要文件`type`将被替换为`trino`、[文档](https://docs.getdbt.com/reference/warehouse-profiles/trino-profile#set-up-a-trino-target)。

## 项目:

在 [dbt 项目文件](https://docs.getdbt.com/reference/dbt_project.yml)、 [dbt_project.yml](https://github.com/victorcouste/trino-dbt-demo/blob/main/dbt_project.yml) 中，我们:

*   使用 trino 配置文件
*   为 BigQuery 目录 **bigquery_catalog** 和模式(数据集) **bigquery_schema** 定义变量
*   为输出设置默认 PostgreSQL 目录和模式(在模型下)

```
name: 'trino_project'
version: '1.0.0'
config-version: 2profile: 'trino'source-paths: ["models"]
analysis-paths: ["analysis"]
test-paths: ["tests"]
data-paths: ["data"]
macro-paths: ["macros"]
snapshot-paths: ["snapshots"]target-path: "target"
clean-targets:
    - "target"
    - "dbt_modules"vars:
  bigquery_catalog: bigquery
  bigquery_schema: data_prepmodels:
  trino_project:
      materialized: table
      catalog: postgresql
      schema: public
```

我们还需要定义一个 [dbt 宏](https://docs.getdbt.com/docs/building-a-dbt-project/jinja-macros#macros)来改变 dbt 在模型中生成和使用新模式的方式(BigQuery 和 PostgreSQL 之间的变化)。宏文件[generate _ schema _ name . SQL](https://github.com/victorcouste/trino-dbt-demo/blob/main/macros/generate_schema_name.sql)。

```
{% macro generate_schema_name(custom_schema_name, node) -%} {%- set default_schema = target.schema -%}
    {%- if custom_schema_name is none -%} {{ default_schema }} {%- else -%} {{ custom_schema_name | trim }} {%- endif -%}{%- endmacro %}
```

## 型号:

在 [customers.sql](https://github.com/victorcouste/trino-dbt-demo/blob/main/models/customers.sql) 模型中，您可以看到我们:

1.  从 jaffle_shop_orders BigQuery 表中计算第一个和最后一个订单日期以及每个客户的订单数。
2.  将前面的结果与 PostgreSQL jaffle _ shop _ customers 表连接。
3.  将结果写入新的 customers PostgreSQL 表中。

# 使用项目

[用`./bin/launcher start`启动 Trino 服务器](https://trino.io/docs/current/installation/deployment.html#running-trino)

Trino 将默认监听 8080 端口。

对于 dbt，运行以下命令:

*   `dbt --version`检查 dbt 是否安装了 presto-dbt 插件。
*   `dbt debug`检查 dbt 项目和与 Presto 的连接。
*   `dbt seed`加载 PostgreSQL 中的[jaffle _ shop _ customers . CSV](https://github.com/victorcouste/trino-dbt-demo/blob/main/data/jaffle_shop_customers.csv)文件。
*   `dbt run`要运行[客户](https://github.com/victorcouste/trino-dbt-demo/blob/main/models/customers.sql)模型，使用聚合的 BigQuery 表进行连接，并创建客户 PostgreSQL 表。
*   `dbt test`测试 customers 表中两列的数据质量。

可以打开 8080 端口上启动的 Trino Web UI([http://localhost:8080](http://localhost:8080/))来检查和监控 dbt 运行的 SQL 查询。

# 扩展项目

对于输入数据集，您可以想象使用 Google BigQuery 公共数据集 [bigquery-public-data](https://cloud.google.com/bigquery/public-data) 。

您还可以使用以下命令直接在 SQL 模型文件中更改 Trino 目录和模式:

```
{{
    config(
        catalog="bigquery",
        schema="data_prep"
     )
}}
```

最后，您可以连接到另一个现有的 Trino 或[star burst Enterprise](https://www.starburst.io/platform/starburst-enterprise/)(Trino Enterprise edition)部署或集群，以获得更高的可扩展性、安全性和性能。

玩得开心！