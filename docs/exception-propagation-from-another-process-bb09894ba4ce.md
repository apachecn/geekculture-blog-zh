# 来自另一个进程的异常传播

> 原文：<https://medium.com/geekculture/exception-propagation-from-another-process-bb09894ba4ce?source=collection_archive---------23----------------------->

将*异常*处理回父进程是非常困难的，甚至是不可能的。

简单的有用，但是很多其他的没用。

例如，`CalledProcessError`是不可选择的(我猜，这是因为`stdout`、`stderr` 数据成员)。

这意味着，如果子进程抛出未被捕获的`CalledProcessError`，它将传播到父进程，但传播将失败，这显然是因为 Python 本身的缺陷。这将导致 *pool.join()永远停止——从而导致内存泄漏！*参见[https://stack overflow . com/questions/15314189/python-multi processing-pool-hangs-at-join](https://stackoverflow.com/questions/15314189/python-multiprocessing-pool-hangs-at-join)和[https://bugs.python.org/issue9400](https://bugs.python.org/issue9400)

因此，我创建了`GuardedWorkerException` 上下文管理器来缓解这个问题。

源代码你可以在这里找到。它是我的 AlexBerUtils 项目的一部分。

您可以从 PyPi 安装 AlexBerUtils:

`python3 -m pip install -U alex-ber-utils`

参见[这里的](https://github.com/alex-ber/AlexBerUtils)了解更多关于如何安装的详细说明。

让我们来看一些具有多进程计算的应用程序的代码存根。

假设您编写了一些脚本或应用程序，打包并安装为 YourApp(也就是说，它安装在 site-packages 或 venv 中)。大概是这样的:

![](img/422bc98791324545648fe88892f2a875.png)

`app.py`的代码:

**注意:**这里我使用的是 *init_app_conf* 模块。你可以在这里阅读[。](/analytics-vidhya/my-major-init-app-conf-module-1a5d9fb3998c)

**注意:**通常，在脚本/应用程序的`main()`函数的末尾有以下几行:

看这里[为我的替代设置制作更多的文件相对路径](https://alex-ber.medium.com/making-more-yo-relative-path-to-file-to-work-fbf6280f9511)。你可以在下面的附录中找到完整的代码。

真正有趣的部分发生在`compute_parallel()`和`prepare_parallel()`内部。注意，`prepare_parallel()`在单个子流程中运行。

基本上，`compute_parallel()`和`prepare_parallel()`是分开运行的。因此，如果异常发生，它将传播到主进程，并可能挂起我们的应用程序。

如果我们将这些函数的代码包装在`GuardedWorkerException`上下文管理器中，它将防止这种挂起。

# GuardedWorkerException 是如何工作的？

先来看看`prepare_parallel()`功能。

此外，导入，所有的函数体都封装在

```
with GuardedWorkerException(logger=logger):
   #...
```

记录器参数是可选的。如果未通过，则使用`sys.stderr`代替。

在我上面的代码中，日志是在`global_init()`函数中配置的。这个全局函数实际上也是初始化的全局`logger`变量，它被传递给`GuardedWorkerException`并用于日志记录。

基本上，如果任何`Exception`(注:`not BaseException`)被引发，它将被记录，到`new Exception(‘Worker failed**’**)`时将被引发。

这样做是为了暂停常规执行，而是为了对流程传播异常使用“良好行为”。我确信上述异常不会中断程序执行或导致内存泄漏。

如果`with`块中的代码正常返回，那就没有什么有趣的事情发生。

我们对`GuardedWorkerException`有什么灵活性？

我们来看看`compute_parallel()`功能。

此外，导入，所有的函数体都封装在

```
with GuardedWorkerException(logger=logger, suppress=True):
    #...
```

基本上，如果任何`Exception`(注意:`not BaseException`)被引发，它将被记录，并且`Exception`本身将被抑制，不会引发任何东西，就像块执行正常退出一样。

`suppress`参数的默认值为`False`。

`default_exc_message` 参数的默认值为“工人故障”。如果您在此处提供另一个值，则引发的消息`Exception`将为`default_exc_message`。

如果您有一些并行处理的不相关计算，这是有意义的。如果你在一个计算中有异常，你不想中止并行的计算，你想完成所有的事情(有一些迹象表明你只有部分结果是个好主意，但是你可以在所有的`compute_parallel()`计算完成后再做)。

注意:你也可以实现重试机制。最简单的实现如下:

**注意:**我已经省略了被传递的参数(`compute_parallel()`通常会有一些参数，这些参数将被传递给`YourClass`的`__init__()`方法。

因此，在第 6–8 行中，我们实例化并运行了`YourClass.`，如果这个代码块引发了`Exception`，我们将等待一段时间，然后再次重试。任何重试失败都将被禁止。

**注意:**如果我们有一些严重的问题(`BaseException`被抛出，它不会被缓存；这是通过设计实现的)。

## 附录

一些“外部主要”:

# 另请参见:

*   [集成 Python 的日志和警告包](/analytics-vidhya/integrating-pythons-logging-and-warnings-packages-7ffd6f65e02d)。
*   `[*fixabscwd()*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py#L12)`功能在`[*mains*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py)` 模块或[制作相对路径的文件才能工作](/@alex_ber/making-relative-path-to-file-to-work-d5d0f1da67bf)。
*   `[*mains*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py)` 模块中的`[*fix_retry_env*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py#L237)*()*` 函数或[Make path to file on Windows works on Linux。](https://alex-ber.medium.com/make-path-to-file-on-windows-works-on-linux-402ed3624f66)
*   `[*mains*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py)` 模块中的`[FixRelCwd()](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py#L263)`功能或[使更多到文件的相对路径工作](https://alex-ber.medium.com/making-more-yo-relative-path-to-file-to-work-fbf6280f9511)
*   `[*mains*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py)` 模块中的`[GuardedWorkerException()](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py#L295)`函数或[异常从另一个进程](https://alex-ber.medium.com/exception-propagation-from-another-process-bb09894ba4ce)传播
*   `[*files*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/files.py)` 模块中的`[join_files()](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/files.py#L9)`功能或[加入文件](https://alex-ber.medium.com/join-files-cc5e38e3c658)。
*   [*stdLogging*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/stdlogging.py) 模块，或[我的 stdLogging 模块](https://alex-ber.medium.com/stdlogging-module-d5d69ff7103f)