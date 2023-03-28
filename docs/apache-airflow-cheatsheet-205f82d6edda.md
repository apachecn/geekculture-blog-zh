# 阿帕奇气流备忘单

> 原文：<https://medium.com/geekculture/apache-airflow-cheatsheet-205f82d6edda?source=collection_archive---------3----------------------->

![](img/cf1e66a0039a63b193e3af4f5c452a5d.png)

Photo by [Jason Blackeye](https://unsplash.com/@jeisblack?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Apache Airflow 是一个以编程方式创作、调度和监控数据管道的系统。阿帕奇气流 1.0.0 版本发布至今已有六年。数月的编码、修复、部署到全球数百家公司的本地服务器和云中。我们克服了许多困难，这里尝试把一些结论放在一个地方，帮助其他人节省时间，使使用气流更容易。

*如果您会发现一些陈述不准确，请不要犹豫，评论，它将尽快修复。*

# 一般

1.  从这里使用官方 Docker-image[https://hub.docker.com/r/apache/airflow](https://hub.docker.com/r/apache/airflow)很容易启动你当地的气流环境
2.  Airflow API 文档有助于了解您可以使用什么工具将外部系统与 Airflow 集成。请在这里阅读[https://air flow . Apache . org/docs/Apache-air flow/stable/security/API . html](https://airflow.apache.org/docs/apache-airflow/stable/security/api.html)
3.  阅读`airflow.cfg`(里面有所有的评论)来了解更多关于如何配置你的气流装置是很好的。请花些时间浏览一下项目回购，了解它是如何在 https://github.com/apache/airflow 建造的。
4.  Airflow(以及我可爱的 Apache 超集)基于 Flask 和 Flask App Builder (FAB)框架之上。这意味着您有许多可定制的功能，例如，您自己的安全管理器。你可以在这里阅读[https://medium . com/geek culture/custom-security-manager-for-Apache-superset-c 91 f 413 A8 be 7](/geekculture/custom-security-manager-for-apache-superset-c91f413a8be7)。

# 扫描文件和 DAG 配置

1.  如果你想让调度程序跳过 DAG 文件夹中的文件，就不要在文件中使用单词`airflow`和`dag`。默认情况下，Airflow scheduler 仅扫描文本中包含这些单词的文件。如果你想改变默认行为，只需在你的`airflow.cfg`中使用`DAG_DICOVERY_SAFE_MODE=False`
2.  另一种方法是通过 DAGs 文件夹根目录中的`.airflowignore`文件忽略不必要的文件。其工作原理与`.gitignore`相同
3.  请注意，如果您有不同的 Dag 使用相同的`dag_id`，将不会出现错误。Web 用户界面将一次显示一个随机 DAG。
4.  这可能不直观，但`start_date`是 DAG 的可选参数。它对于每个任务都是必须的，但是由于某种原因，它在 DAG 的不同任务中可能是不同的，这是违反直觉的。
5.  DAG 的默认 schedule_interval 是`timedelta(days=1)`
6.  如果你在使用永无止境的 Dag 时遇到问题，那么`dagrun_timeout`参数可以帮助你。

# DAG 运行

1.  `catchup`参数默认设置为`True`。这意味着每次取消暂停 DAG 时，所有错过的 DAG 运行都会立即开始。最佳实践是为每个 DAG 将此设置为`False`。甚至，您可能想要在`airflow.cfg`中设置`CATCHUP_BY_DEFAULT=False`来改变所有 Dag 的默认行为。无论如何，即使有了`catchup=False`，一个 dagrun 仍然会在解暂停后运行。
2.  DAG 的第一个 dagrun 将在`start_date + schedule_interval`触发，实际上是在`execution_date`。
3.  您可以使用`pendulum`模块创建时区感知 Dag。在版本`≥1.10.7`中，如果为 DAG 指定了时区，cron 表达式将遵循夏令时(DST)，但 timedelta 对象不会这样做。顺便说一下，在`1.10.7`之前的 Airflow 版本中，一切都是反过来的——time delta 对象尊重 DST，但 cron 表达式不尊重 DST。
4.  不要把气流当成多功能的东西。它是一个非常强大的流程编排器，或者像有些人说的 ***它是一个服用了类固醇的克罗恩*** 。使用它来触发气流系统之外的作业。例如，如果您想将数据从 PgSQL 或 MySQL 加载到 Clickhouse DB —如果适用，使用它自己的机制从外部表加载数据，而不是编写您自己的自定义操作符或使用带有`ClickHouseOperator`的 PostgresOperator。有些人认为`DockerOperator`和`KubernetesPodOperator`应该是您在生产环境中使用的唯一两种类型的操作符。
5.  如果您使用 AWS 环境，并且需要在新文件到达 S3 存储桶时启动作业，那么最好使用 AWS Lambda 来触发 dagrun，而不是使用传感器。这里有一篇关于如何使用 Lambdas [的有用文章 https://medium . com/swlh/AWS-lambda-run-your-code-for-free-1c 7 fa 6714 ee 9](/swlh/aws-lambda-run-your-code-for-free-1c7fa6714ee9)。

# 变量和连接

1.  隐藏某些变量的值是可能的。您可以在`airflow.cfg`的`DEFAULT_SENSITIVE_VARIABLE_FIELDS`中找到哪些字段将被屏蔽。
2.  使用`deserialize_json=True`作为`Variable.get()`的参数，可以从 JSON 变量值中得到一个`dict()`对象。它有助于减少与气流数据库的连接数量。
3.  将`Variable.get()`语句放在函数定义中，而不是在主 DAG 代码中使用它们，会让您受益匪浅。否则，调度程序将在每次扫描文件系统寻找新 Dag 时创建数据库连接。
4.  通过使用环境变量，可以减少数据库连接，而无需将`Variable.get()`语句移到函数定义中。以`AIRFLOW_VAR_*`开头的 Env 变量在 UI 变量之前被处理，如果有变量的话就停止进一步的搜索。请注意，env 变量在 UI 中是不可见的。
5.  另一种方法是使用 Jinja 模板。任务的模板化字段将仅在任务运行时处理，而不会在调度程序定期扫描 DAG 文件夹时处理。使用`op_args=["{{ var.json.varname.jsonkey }}"]`读取模板字段中的变量
6.  您可以使用环境变量以及名称中的`AIRFLOW_CONN_*`模式创建气流连接。
7.  从版本`1.10.10`开始，可以使用自定义的秘密后端，如 AWS 秘密管理器或 GCP 秘密管理器。

# 任务执行参数

1.  不使用 XCOM 在任务间传输*数据*，而是鼓励您通过 XCOM 传输*元数据*。
2.  task 的`depends_on_past`参数防止在先前 dagrun 中的同一任务被标记为成功或失败之前执行该任务。
3.  `wait_for_downstream`参数允许任务仅在同一任务及其直接下游任务在之前的 dagrun 中完成时执行。
4.  任务`priority_rule='downstream'`(默认)通过对所有下游任务的优先级求和来计算绝对任务优先级权重。例如，当您运行 2 个 dagrun 并且任务池大小为 1 时，dag run 的执行将并行进行(任务从一个 Dag run 开始，然后从另一个 Dag run 开始)。
5.  任务`priority_rule='upstream'`通过合计所有上游任务的优先级来计算绝对任务优先级权重。例如，当有 2 个 dagrun 正在运行，并且任务池的大小为 1 时，第二个 dagrun 中的任务只有在第一个 Dag run 中的最后一个任务完成后才会执行。
6.  `execution_timeout`参数对于长时间运行的任务很有用。
7.  `trigger_rule`就是为你准备的，如果你想改变任务的行为默认是哪个`ALL_SUCCESS`。可用值的完整列表可在此处找到[https://github . com/Apache/air flow/blob/main/air flow/utils/trigger _ rule . py](https://github.com/apache/airflow/blob/main/airflow/utils/trigger_rule.py)

# 并行执行

1.  `parallelism`(默认 32)—整个气流实例中可以同时执行多少个任务(`airflow.cfg`参数)。
2.  `dag_concurrency`(默认为 16)—DAG 中可以同时执行多少个任务(DAG 参数)。
3.  `max_active_runs_per_dag`(默认为 16)—可以同时执行多少个 Dag(`airflow.cfg`参数)。
4.  `max_active_runs` —可以同时执行多少个 DAG(DAG 参数)。
5.  `max_queued_runs_per_dag`单个 DAG 的最大排队 DAG 运行数，调度程序不会创建更多 DAG 运行(`airflow.cfg`参数)。

# 传感器

1.  `poke_interval` —传感器检查状况的频率。
2.  带有`mode='poke'`的传感器占用池中的一个插槽，直到满足条件。
3.  带`mode='reschedule'`的传感器仅在检查条件时占用池中的一个槽，并在下一次戳之前释放该槽。
4.  `timeout`带`soft_fail=True`的参数(默认为 7 天)可以将传感器设置为`skipped`状态，而不是`failed`状态。
5.  `exponental_backoff=True`将固定的`poke_interval`替换为更动态的。

# DAG 依赖性

1.  `ExternalTaskSensor`在另一个 DAG 中监视特定的 task_id 及其状态。
2.  `TriggerDagRunOperator`触发另一个 DAG，并且可以等待它被执行。

我希望您喜欢这篇文章，这篇信息将帮助您构建高效的数据管道。如果有，请在 [Medium](/@agordienko) 、 [GitHub](https://github.com/aleksandrgordienko) 、 [Twitter](https://twitter.com/data_diving) 、 [LinkedIn](https://www.linkedin.com/in/aleksandrgordienko/) 上关注我。