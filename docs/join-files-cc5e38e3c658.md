# 加入文件

> 原文：<https://medium.com/geekculture/join-files-cc5e38e3c658?source=collection_archive---------17----------------------->

假设您有一些多线程/多进程应用程序，其中每个线程/进程创建一些文件(每个线程/进程创建**不同的**文件)，您想要将它们合并到一个文件中。创建的文件有一些命名约定，例如`output*.txt`，结果文件应该是`output.txt`。

**注意**:例如，你可以用线程/进程名来代替星号，这样就足够独特了。

现在，在主进程/线程中，您想要创建一个最终文件，该文件将包含所有已创建文件的内容(它们由文件掩码标识，如`output*.txt`)到一个最终文件(如`output.txt`)。

**注意:**如果您期望有一些`output-SpawnPoolWorker-2.txt`而它却不存在，您可能想要在最终文件中添加一些指示。下面的代码不会自动执行。如果需要，您可以在加入结果之前自己创建带有错误指示的`output-SpawnPoolWorker-2.txt`。

为了做到这一点，我创建了`join_files`函数。

源代码你可以在这里找到。它是我的 AlexBerUtils 项目的一部分。

您可以从 PyPi 安装 AlexBerUtils:

`python3 -m pip install -U alex-ber-utils`

关于如何安装的更多详细说明，请参见[这里的](https://github.com/alex-ber/AlexBerUtils)。

让我们看看应用程序的一些代码存根:

`app.py`的代码:

**注意:**这里我使用的是 *init_app_conf* 模块。你可以在这里阅读。

**注意:**通常，在脚本/应用程序的`main()`函数的末尾有以下几行:

看这里[为我的替代设置制作更多的文件相对路径](https://alex-ber.medium.com/making-more-yo-relative-path-to-file-to-work-fbf6280f9511)。

要理解`GuardedWorkerException`，请参见[来自另一个进程的异常传播](https://alex-ber.medium.com/exception-propagation-from-another-process-bb09894ba4ce)。

你可以在另一个进程的[异常传播中的`Appendix`上查找缺失的代码部分。](https://alex-ber.medium.com/exception-propagation-from-another-process-bb09894ba4ce)

这里有两个有趣的部分:`_write_partial_output()`和`join_files()`。

`_write_partial_output()`应该真的是某个类的一部分。`account_id`、`user_id`应为计算值。第一部分，第 67-74 行只是一些关于如何为每个进程/线程生成唯一(足够)文件名的*示例*代码——它从配置字典中获取不带后缀的文件名，并附加一些唯一部分和保留后缀(也保留目录)。第二部分，第 75–76 行是将内容存储为 CSV 的简单方法。我要重复一遍，这只是样本，并不打算成为通用的可重用代码片段。

`join_files()` —在我们调用这个方法之前，我们从配置字典中检索完整的文件名(例如，`/tmp/output.txt`)。`join_files()`会查看文件被指出的目录(例如`/tmp/`)；它的文件名将是结果文件的文件名(例如`output.txt`),并且`join_files()`将在`/tmp/`中的`output*.txt`上查找所有创建的文件(这在`_write_partial_output()`中完成)。

# `join_files()`如何工作？

1.  首先，它创建了引用输出文件的类似文件的对象(用于写入)。
2.  然后迭代具有相似掩码的文件(如`output*.txt`)并创建类似文件的对象(用于读取)。
    **实现注意:**我用的是`pathlib.path.glob()`。
3.  然后，它将数据从类似读取文件的对象逐块复制到类似写入文件的对象。
    **实现注意:**我用的是`shutil.copyfileobj()`。

源代码你可以在这里找到。它是我的 AlexBerUtils 项目的一部分。

您可以从 PyPi 安装 AlexBerUtils:

`python3 -m pip install -U alex-ber-utils`

参见[此处](https://github.com/alex-ber/AlexBerUtils)了解更多关于如何安装的详细说明。

# 另请参见:

*   [集成 Python 的日志和警告包](/analytics-vidhya/integrating-pythons-logging-and-warnings-packages-7ffd6f65e02d)。
*   `[*mains*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py)` 模块中的`[*fixabscwd()*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py#L12)`功能或[制作文件的相对路径来工作](/@alex_ber/making-relative-path-to-file-to-work-d5d0f1da67bf)。
*   `[*mains*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py)` 模块中的`[*fix_retry_env*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py#L237)*()*` 功能或[在 Windows 上制作文件路径在 Linux 上工作。](https://alex-ber.medium.com/make-path-to-file-on-windows-works-on-linux-402ed3624f66)
*   `[FixRelCwd()](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py#L263)`功能在`[*mains*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py)` 模块或[模块中多以相对路径文件来工作](https://alex-ber.medium.com/making-more-yo-relative-path-to-file-to-work-fbf6280f9511)
*   `[*mains*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py)` 模块中的`[GuardedWorkerException()](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py#L295)`函数或[异常从另一个进程](https://alex-ber.medium.com/exception-propagation-from-another-process-bb09894ba4ce)传播
*   `[*files*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/files.py)` 模块中的`[join_files()](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/files.py#L9)`功能或[加入文件](https://alex-ber.medium.com/join-files-cc5e38e3c658)。
*   [*stdLogging*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/stdlogging.py) 模块，或[我的 stdLogging 模块](https://alex-ber.medium.com/stdlogging-module-d5d69ff7103f)