# 使文件的相对路径起作用

> 原文：<https://medium.com/geekculture/making-relative-path-to-file-to-work-d5d0f1da67bf?source=collection_archive---------0----------------------->

## 而不用担心当前的工作目录(Python)。

在之前的文章中，我描述了我配置日志的方法。其中一个缺陷是，如果我想使用相对于路径的*来记录配置文件，我应该从同一个文件夹运行我的应用程序/脚本。*

**注意:**日志配置文件不特殊。当你使用*相对*路径到*任何*文件时，问题就出现了。

我写了一个小的实用函数来克服这个限制。您可以从任何工作目录运行您的应用程序/脚本，这个函数会将 cwd 固定到预期的目录，这样*相对*路径就会起作用。更多详情见下文。

源代码你可以在这里找到。它是我的 [AlexBerUtils](https://github.com/alex-ber/AlexBerUtils) s 项目的一部分。

您可以从 PyPi 安装 AlexBerUtils:

`python3 -m pip install -U alex-ber-utils`

关于如何安装的更多详细说明，请参见此处的。

这是一个用法示例:

现在，我建议您在完成基本的日志记录配置后，将这段代码片段(因为，这个函数使用 logger 否则，你会丢失这些消息)，但是在你使用任何*相对*路径之前。它可以是*到记录配置文件的相对*路径，或者例如到`.env`文件的路径。

您的代码应该如下所示:

# `fixabscwd`是如何运作的？

基本上，它在`__main__`模块中寻找`__file__`属性，从那里获取目录并为其设置 cwd(通过调用`os.chdir`)。

如果在 REPL 或 IPython 笔记本中调用这个函数，它什么也不做。

这个函数没有从冻结的 python 脚本(使用 py2exe 冻结)中测试过，但是它应该可以工作(不做任何事情)。

尽情享受吧！

# 另请参见:

*   `[*importer*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/importer.py)` 模块或者[如何编写轻松可定制的代码？](/analytics-vidhya/how-to-write-easily-customizable-code-8b00b43406b2)
*   `[*mains*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py)` 模块中的`[*fixabscwd()*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py#L12)`功能或者[制作文件的相对路径来工作](/@alex_ber/making-relative-path-to-file-to-work-d5d0f1da67bf)。
*   `[*mains*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py)` 模块或[中的](https://alex-ber.medium.com/make-path-to-file-on-windows-works-on-linux-402ed3624f66)`[*fix_retry_env*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py#L237)*()*` 函数在 Windows 上生成文件路径在 Linux 上有效。
*   [*解析器*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/parsers.py) 模块或[描述一个最老的 AlexBerUtils 项目](/analytics-vidhya/my-parser-module-429ed1457718)
*   [*ymlparser*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/ymlparsers.py)*模块或者[模块的低级 API 模块对其他模块的描述](/analytics-vidhya/my-ymlparsers-module-88221edf16a6)*
*   *[*init_app_conf*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/init_app_conf.py)模块，或者[我的专业 init _ app _ con 模块](/analytics-vidhya/my-major-init-app-conf-module-1a5d9fb3998c)*
*   *[*部署*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/deploys.py) 模块，或者[我的部署模块](/analytics-vidhya/my-deploys-module-26c5599f1b15)*
*   *[*电子邮件*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/emails.py) 模块，或者[我的电子邮件模块](/analytics-vidhya/my-emails-module-3ad36a4861c5)*
*   *[*processinvokes*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/processinvokes.py)*模块，或[我的 processinvokes 模块](/analytics-vidhya/my-processinvokes-module-de4d301518df)**
*   **[stdLogging](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/stdlogging.py) 模块，或 [My stdLogging 模块](https://alex-ber.medium.com/stdlogging-module-d5d69ff7103f)**

# **另请参见:**

*   **[集成 Python 的日志和警告包](/analytics-vidhya/integrating-pythons-logging-and-warnings-packages-7ffd6f65e02d)。**
*   **`[*mains*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py)` 模块中的`[*fixabscwd()*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py#L12)`功能或[制作工作文件的相对路径](/@alex_ber/making-relative-path-to-file-to-work-d5d0f1da67bf)。**
*   **`[*mains*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py)` 模块中的`[*fix_retry_env*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py#L237)*()*` 功能或者[在 Windows 上制作文件路径在 Linux 上工作。](https://alex-ber.medium.com/make-path-to-file-on-windows-works-on-linux-402ed3624f66)**
*   **`[FixRelCwd()](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py#L263)`功能在`[*mains*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py)` 模块或[模块中制作多到相对路径的文件才能工作](https://alex-ber.medium.com/making-more-yo-relative-path-to-file-to-work-fbf6280f9511)**
*   **`[*mains*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py)` 模块中的`[GuardedWorkerException()](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/mains.py#L295)`函数或者来自另一个进程的[异常传播](https://alex-ber.medium.com/exception-propagation-from-another-process-bb09894ba4ce)**
*   **`[join_files()](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/files.py#L9)`功能在`[*files*](https://github.com/alex-ber/AlexBerUtils/blob/master/alexber/utils/files.py)` 模块或[模块中加入文件](https://alex-ber.medium.com/join-files-cc5e38e3c658)。**