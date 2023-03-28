# 我的标准日志模块

> 原文：<https://medium.com/geekculture/stdlogging-module-d5d69ff7103f?source=collection_archive---------32----------------------->

# AlexBerUtils 项目中的描述模块

这是瘦适配器层，将 stdout/stderr(或任何其他类似流的对象)重定向到标准 Python 的 logger。

源代码你可以在这里找到[。它是我的 AlexBerUtils 项目的一部分。](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/emails.py)

您可以从 PyPi 安装 AlexBerUtils:

```
python3 -m pip install -U alex-ber-utils
```

你可以选择安装一些依赖项，比如 jinja2。最简单的方法是跑步:

```
python3 -m pip install alex-ber-utils[yml]
```

**注意**同时 [hiyapyco](https://github.com/zerwes/hiyapyco) 至少应为 0.4.16。

然而，这并不是严格要求的。

关于如何安装的更多详细说明，请参见此处的。

# 高级描述

该模块基于[https://github . com/FX-Kirin/py-STD logging/blob/master/STD logging . py](https://github.com/fx-kirin/py-stdlogging/blob/master/stdlogging.py)参见[https://github.com/fx-kirin/py-stdlogging/pull/1](https://github.com/fx-kirin/py-stdlogging/pull/1)。我在这里遇到过关于这个模块的[https://stack overflow . com/questions/47325506/making-python-loggers-log-all-stdout-and-stderr-messages](https://stackoverflow.com/questions/47325506/making-python-loggers-log-all-stdout-and-stderr-messages)。引用:“但是要小心捕获 stdout，因为它非常脆弱”

我有一些第三方模块(即 webpy ),它使用了`stdout` 和`stderr` ,但没有使用标准 Python 的日志机制。我的应用程序实际上是在 docker 内部运行的，所以我希望能够将这些输出重定向到标准的日志记录机制(这使我获得了最大的灵活性——例如，我可以在`stdout`和日志文件中保存我的日志，日志文件将存储在挂载的卷上)。

模块恰恰给了我这种能力。在我的特定用例中，只重定向`stderr`就足够了。正如我之前说过的，我正在 Docker 内部运行。我在用`tty`跑步。因为[https://UNIX . stack exchange . com/questions/616616/separate-stdout-and-stderr-for-docker-run](https://unix.stackexchange.com/questions/616616/separate-stdout-and-stderr-for-docker-run)所以只为`stderr`做就够了。

# 使用示例:

配置文件(简化):

**注意:**我正在使用[我的 init_app_conf 模块](/analytics-vidhya/my-major-init-app-conf-module-1a5d9fb3998c)来解析这个文件。这不是必需的，您可以根据需要初始化日志模块。

如果你想改编`stdout`，你应该写:

如果您想重定向来自`stdout`和`stderr`的输出(它们是不同的流)，您可以在代码中包含这两行。或者，您可以在程序启动时将`stderr` 重定向到`stdout`(这就是 Docker 所做的，如果您在`docker run`命令中有`-t`选项)。

您还可以提供用于回显输出的`logger_level`。默认为`logging.ERROR`。

# 高级用法:

您可以指定自己的适配器类。例如:

这指定了默认适配器类。您可以提供自己的实现。它可以是上面代码中的`str`或`class`。您的实现可以扩展默认协议，或者只实现相同的协议。

# 另请参见:

*   `[*importer*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/importer.py)` 模块或者[如何编写轻松可定制的代码？](/analytics-vidhya/how-to-write-easily-customizable-code-8b00b43406b2)
*   `[*mains*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py)` 模块中的`[*fixabscwd()*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py#L12)`功能或者[制作文件的相对路径来工作](/@alex_ber/making-relative-path-to-file-to-work-d5d0f1da67bf)。
*   `[*mains*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py)` 模块或[中的](https://alex-ber.medium.com/make-path-to-file-on-windows-works-on-linux-402ed3624f66)`[*fix_retry_env*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py#L237)*()*` 函数在 Windows 上生成文件路径在 Linux 上有效。
*   [*解析器*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/parsers.py) 模块或[描述一个最老的 AlexBerUtils 项目](/analytics-vidhya/my-parser-module-429ed1457718)
*   [*ymlparser*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/ymlparsers.py)*模块或者[模块的低级 API 模块对其他模块的描述](/analytics-vidhya/my-ymlparsers-module-88221edf16a6)*
*   *[*init_app_conf*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/init_app_conf.py)模块，或者[我的专业 init _ app _ con 模块](/analytics-vidhya/my-major-init-app-conf-module-1a5d9fb3998c)*
*   *[*部署*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/deploys.py) 模块，或者[我的部署模块](/analytics-vidhya/my-deploys-module-26c5599f1b15)*
*   *[*电子邮件*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/emails.py) 模块，或者[我的电子邮件模块](/analytics-vidhya/my-emails-module-3ad36a4861c5)*
*   *[*processinvokes*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/processinvokes.py)*模块，或 [My processinvokes 模块](/analytics-vidhya/my-processinvokes-module-de4d301518df)**
*   **[*stdLogging*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/stdlogging.py) 模块，或 [My stdLogging 模块](https://alex-ber.medium.com/stdlogging-module-d5d69ff7103f)**