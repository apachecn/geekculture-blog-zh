# 硒载体

> 原文：<https://medium.com/geekculture/selenium-support-19330843c63a?source=collection_archive---------13----------------------->

硒伞项目的有用功能。

我已经开始使用 Selenium 的相关产品，并且发现文档具有误导性，它并没有促进最佳实践。此外，许多组件存在多年都无法修复的内存/资源泄漏。因此，我创建了一个实用程序项目，将我对 Selenium 的“修复”封装起来，并推广最佳实践。

我将列出我的库的一些功能:

*   创建/销毁`BmpDaemon`(又名 browsermobproxy。服务器)。
*   创建/销毁`BmpProxy` (又名 browsermobproxy。客户端)。
*   创建/销毁`SeleniumWebDriver` (例如**selenium . web driver . chrome . web driver)。** *(可以是任何支持的浏览器)。*
*   截图。
*   准备浏览器的数据目录以供使用。
*   启用浏览器下载文件。
*   以 har 格式捕获网络。
*   正在等待页面加载。
*   同步点击(在按钮上)。
*   等待谷歌 Chrome 完成下载文件 *(Chrome 专用)*。
*   等待显示。

源代码你可以在这里找到。它是我的 AlexBerUtils 项目的一部分。

您可以从 PyPi 安装 SeleniumSupport:

`python -m pip install -U selenium-support`

关于如何安装的更多详细说明，请参见此处的。

硒元素-4 还在开发中，见[硒元素 4 是什么？自动化浏览器测试的最新进展](https://testguild.com/selenium-4/)，所以这个故事是关于 Selenium 3 的。

开始使用 Selenium 的标准代码如下所示:

```
path = ‘bin/browsermob-proxy’ #your path to browsermob-proxy
server = Server(path)
server.start()
proxy = server.create_proxy()
```

[https://medium . com/@ jiurdqe/how-to-get-JSON-response-body-with-selenium-amd-browser mob-proxy-71f 10335 c66](/@jiurdqe/how-to-get-json-response-body-with-selenium-amd-browsermob-proxy-71f10335c66)

另一个稍微有点变体: **settings.py:**

```
**class** Config(object):
    CHROME_PATH = **'/Library/Application Support/Google/chromedriver76.0.3809.68'** BROWSERMOB_PATH = **'/usr/local/bin/browsermob-proxy-2.1.4/bin/browsermob-proxy'

class** Docker(Config):
    CHROME_PATH = **'/usr/local/bin/chromedriver'**import Server from selenium
import contextlib
from settings import Config as config@contextlib.contextmanager 
def browser_and_proxy(): 
    server = Server(config.BROWSERMOB_PATH) 
    server.start() 
    proxy = server.create_proxy()
    #...
    *# Set up Chrome* option = webdriver.ChromeOptions()
    option.add_argument(**'--proxy-server=%s'** % proxy.proxy) prefs = {**"profile.managed_default_content_settings.images"**: 2}
    option.add_experimental_option(**"prefs"**, prefs)
    option.add_argument(**'--headless'**)
    option.add_argument(**'--no-sandbox'**)
    option.add_argument(**'--disable-gpu'**)

    capabilities = DesiredCapabilities.CHROME.copy()
    capabilities[**'acceptSslCerts'**] = **True** capabilities[**'acceptInsecureCerts'**] = **True** path = config.CHROME_PATH
    browser = webdriver.Chrome(options=option,
                           desired_capabilities=capabilities,
                           executable_path=path)

    **try**:
      **yield** browser, proxy
    **finally**:
      browser.quit()
      server.stop()#...**with** browser_and_proxy() **as** (browser, proxy):
        browser.get(**'https://www.airbnb.com/s/Seattle--WA--United-States/homes'**)
#...
```

基于[https://medium . com/@ sshevlyagin/browser mob-proxy-and-selenium-known-when-requests-finish-44d 4b 52851 c8](/@sshevlyagin/browsermob-proxy-and-selenium-knowing-when-requests-finish-44d4b52851c8)

1.  首先，很明显上面的代码片段有严重的内存/资源泄漏——服务器和代理没有关闭。
2.  第二段代码也有(几处)漏洞。最明显的一个——如果`browser.quite()`引发异常，那么`server.stop()`将永远不会被调用，并将导致泄漏。另一个，`proxy`也应该通过调用`proxy.close()`关闭。
3.  `server.stop()`法非同小可的泄密。下面我会详细描述。
4.  非同小可的泄密有`browser.stop()`方法。下面我会详细描述。这种泄漏的性质与第 3 页中的相同

上面一行中的`proxy`是什么类型？

```
proxy = server.create_proxy()
```

不看`create_proxy()`方法的实现，你永远不会猜到它实际上是……`Client`。(`browsermobproxy.Client`更确切的说)。

引用自文档[https://github . com/light body/browser mob-proxy # getting-started-standalone](https://github.com/lightbody/browsermob-proxy#getting-started-standalone):

> *如果您在 Java 应用程序或 Selenium 测试中运行 BrowserMob 代理，请从* [*嵌入式模式*](https://github.com/lightbody/browsermob-proxy#getting-started-embedded-mode) *开始。如果您想从命令行作为独立代理运行 BMP，请从* [*独立*](https://github.com/lightbody/browsermob-proxy#getting-started-standalone) *…* 开始
> 
> *要从命令行以独立模式运行，首先从* [*版本页面*](https://github.com/lightbody/browsermob-proxy/releases) *或* [*下载最新版本，从源代码*](https://github.com/lightbody/browsermob-proxy#building-the-latest-from-source) *构建最新版本。*
> 
> *启动 REST API:*
> 
> `*./browsermob-proxy -port 8080*`
> 
> *然后创建一个代理服务器实例:*
> 
> `*curl -X POST* [*http://localhost:8080/proxy*](http://localhost:8080/proxy)``*{"port":8081}*`
> 
> *“port”是新创建的代理实例的端口，因此配置您的 HTTP 客户机或 web 浏览器，以便在返回的端口上使用代理。有关 REST API 中可用特性的更多信息，请参见*[*REST API 文档*](https://github.com/lightbody/browsermob-proxy#rest-api) *。*

让我们跟随链接。引自[https://github.com/lightbody/browsermob-proxy#rest-api](https://github.com/lightbody/browsermob-proxy#rest-api)

> ***2.1 中新增:*** *LittleProxy 是 REST API 的默认实现。您可以指定* `*--use-littleproxy false*` *来禁用 LittleProxy，以支持传统的基于 Jetty 5 的实现。*
> 
> *首先在 bin 目录下运行* `*browsermob-proxy*` *或* `*browsermob-proxy.bat*` *来启动代理:*
> 
> `*$ sh browsermob-proxy -port 8080
> INFO 05/31 03:12:48 o.b.p.Main - Starting up...
> 2011-05-30 20:12:49.517:INFO::jetty-7.3.0.v20110203
> 2011-05-30 20:12:49.689:INFO::started o.e.j.s.ServletContextHandler{/,null}
> 2011-05-30 20:12:49.820:INFO::Started SelectChannelConnector@0.0.0.0:8080*`
> 
> *一旦启动，* ***在您创建新的代理*** *之前，不会有实际的代理在运行。你可以通过发帖到/proxy:* 来完成
> 
> `*[~]$ curl -X POST http://localhost:8080/proxy
> {"port":8081}*`
> 
> *或者指定您自己的端口:*
> 
> `*[~]$ curl -X POST -d 'port=8089' http://localhost:8080/proxy
> {"port":8089}*`
> 
> *或者，如果在多宿主环境中运行 BrowserMob 代理，指定所需的绑定地址(默认为* `*0.0.0.0*` *):*
> 
> `*[~]$ curl -X POST -d 'bindAddress=192.168.1.222' http://localhost:8080/proxy
> {"port":8086}*`
> 
> *一旦完成，新的代理将在返回的端口上可用。你所要做的就是将一个浏览器指向那个端口上的代理，然后你就可以浏览互联网了。*

# 有点不为人知

```
server.start()
```

这个命令实际上执行了这个*命令*
`browsermob-proxy -port 8080`

注:在 Mac 上其实是`sh browsermob-proxy -port 8080`。

**注意:**在 Windows 上，如果您从非控制台进程中启动子进程，将出现一个控制台窗口(`conhost.exe`)。详情见[https://code . active state . com/recipes/409002-launching-a-subprocess-without-a-console-window/](https://code.activestate.com/recipes/409002-launching-a-subprocess-without-a-console-window/)和[https://docs . python . org/3/library/subprocess . html # subprocess。STARTUPINFO.dwFlags](https://docs.python.org/3/library/subprocess.html#subprocess.STARTUPINFO.dwFlags)

**还要注意:**有效命令将在`conhost.exe`中`browsermob-proxy**.bat** -port 8080` 运行。

我将在下面回到这一点。因此，`server.start()`使用了`subprocess.Popen(self.*command...*)`，其中 command 是上面的变体之一。 ***不会有实际代理*** (你可能以为`browsermob-proxy` 开始是“实际代理”，呵呵，其实没有)。这个命令启动 **BMP *守护进程***——这是一个独立的 Java 应用程序，作为操作系统的一部分。

如何启动 ***实际代理*** ？
`proxy = server.create_proxy()`

其中代理有类型，正如我上面所说的，`Client`(更准确地说是`browsermobproxy.Client`)。

在创建代理时，调用下面的 API:
`curl -X POST [http://localhost:8080/proxy](http://localhost:8080/proxy)`

并且调用返回*端口号*，其中有 ***专用的“实际”代理*** *。*

***注:***

*   最佳实践是为每个**进程分配 ***【实际】代理*** 。返回的*端口号*应该传递给浏览器。**
*   如果你需要在你的应用中打开多个浏览器，最好使用不同的**进程**而不是**线程**。

# 一般说明

在下文中，我将使用以下*命名约定*:

*   *BMP 守护进程—* 指的是以`sh browsermob-proxy -port 8080` *命令的某种变体启动的 Java 程序。*
*   *BMP 代理* —参考`browsermobproxy.Client`类型，这是为实际使用而创建的 ***【实际】代理*** ，每个 Python 进程一个。
*   *Web 驱动*(我不喜欢*驱动*快捷方式；*驱动*在 Selenium 世界*之外有更广泛的含义)——*指“控制”浏览器的代码。这是硒的一部分。实际上，这是我正在使用的硒的主要部分。
*   *许多函数以字典为参数*。您可以用硬编码的参数创建 dict，您可以将参数作为命名变量( *kwargs* )传递，或者您可以将参数放在某个配置文件中并解析它。如果你选择了后者，你基本上有以下选择:

1.  选择你最喜欢的 Yml 解析器并解析成 dict。比如 ruyaml，pyyaml 等。
2.  可以用[我的 ymlparsers 模块](/analytics-vidhya/my-ymlparsers-module-88221edf16a6)。
3.  如果您想根据您正在工作的环境/概要文件(例如 dev、prod)使用不同的值，您可以使用 [my *init_app_conf* 模块](/analytics-vidhya/my-major-init-app-conf-module-1a5d9fb3998c)。

我个人选择的是 [*init_app_conf* 模块](/analytics-vidhya/my-major-init-app-conf-module-1a5d9fb3998c)。

# *BMP 守护进程*

代码:

其中 *dd* 可描述如下:

**注:**关于如何将 yml 文件转换成 dict *dd* 见上面*“许多函数以字典为参数”*一节。

解释是:

是负责启动 BMP 守护进程的上下文管理器。在从上下文管理器的代码块中退出时，BMP 守护程序将被停止，参见`closeBmpDaemon().`

启动 BMP 守护程序基本上是通过以下代码完成的:

```
server = Server(**daemon_init_d)
server.start(**daemon_start_d)
```

其中 *daemon_init_d* 表示 *init* 下面的部分

而 *daemon_start_d* 代表 *start 下面的部分。*

**注:**

*   你可以在你的代码之外手动启动 BMP 守护进程*。或者，您可以将它包装为实际操作系统守护程序，在操作系统启动时进行 startד。*
*   *如果你想以编程方式启动 BMP 守护进程*，例如，使用`BMPDaemon`实用程序，建议从主进程开始。**
*   **如果您想以编程方式使用 BMP 守护程序，您不应该忘记关闭它。更多关于关闭下面。**
*   **如果你真的愿意，你可以像函数调用一样使用`BMPDaemon`工具。在这种情况下，你应该使用`try-finally`块并显式调用`closeBmpDaemon()`函数。更多关于关闭下面。**
*   **如果您有多个应用程序使用 BMP 守护程序，您有两个基本选择:**

1.  **在这些应用程序范围之外启动 BMP 守护程序(可能作为 3 应用程序或操作系统守护程序，或者只是手动启动，见上文)，在一些预定义的端口(默认为 8080)中编写假设 BMP 守护程序已经启动并运行的应用程序代码。**
2.  **每个应用程序将在不同的端口上启动 BMP 守护程序。**

**我想**强调**，从技术上来说，所有应用程序只需要运行一个 BMP 守护程序就足够了(您将为每个应用程序创建 BMP 代理)。**

**就个人而言，我发现第二种选择更容易管理——即拥有多个 BMP 守护进程，每个应用程序一个。这确实是资源的浪费，但是应用程序的生命周期更容易管理，并且您没有一些外部依赖。但是，请注意，在这种情况下，至少在一个应用程序中，您应该显式地提供端口号*。它们最好彼此远离，因为 BMP 代理是作为下一个端口号创建的。***

**举个明显的例子，在上面的代码中，BMP 守护进程将在端口 8090 上启动。第一个 BMP 代理将位于端口 8091，每个后续的 BMP 代理将使用下一个端口号。如果您知道，您将有多达 9 个进程，每个进程都将使用 BMP 代理，BMP 代理将占用端口范围 8091–8099。所以，如果你想再要一个 BMP 守护进程，不能是 8091，最好是*至少 8100(或者 8090 以下)。***

***daemon_init_d —* 这个字典被传递给`[__init__()](https://github.com/AutomatedTester/browsermob-proxy-py/blob/master/browsermobproxy/server.py#L59)`:**

*   ***路径*的默认值为`browsermob-proxy`。(在 Windows 上是`browsermob-proxy.bat`)。如果该文件在 OS 环境变量中不可用，您应该为可执行文件提供显式文件，如上例所示。**
*   **`options['port']`的默认值是 8080。在上面引用的文档中，这是 BMP 守护程序运行时的默认端口。但是，通常这个端口被其他应用程序使用，所以您可能需要更改它。**

***daemon_start_d —* 这个字典被传递给`[start()](https://github.com/AutomatedTester/browsermob-proxy-py/blob/master/browsermobproxy/server.py#L99)`:**

*   **`options['log_path']`的默认值为`os.getcwd()`。这代表目录(没有文件名！)BMP 守护进程的日志将被写入其中。**
*   **`options['log_file']`的默认值为`server.log`。这代表文件名(只有文件名，没有目录路径！)的日志。**
*   **还有一些未记录的值可以传递。请参见[https://github . com/automated tester/browser mob-proxy-py/blob/master/browsermobproxy/server . py # L59](https://github.com/AutomatedTester/browsermob-proxy-py/blob/master/browsermobproxy/server.py#L59)**

**这个上下文管理器也担心关闭 BMP 守护进程。见下面的`closeBmpDaemon()`。同样，如果你把它作为常规函数使用，这种情况不会发生。**

# **关闭 BMP 守护程序**

**如果你使用`*BMPDaemon*`作为上下文管理器，它会担心关闭 BMP 守护进程。如果想自己做，可以调用`closeBmpDaemon()` 函数。**

**您可能想知道为什么调用`bmp_daemon.stop()`是不够的，看起来这个方法是专门为此设计的。**

**首先，有一个公开的问题 [Java 进程没有在 Mac](https://github.com/AutomatedTester/browsermob-proxy-py/issues/76) 中被杀死。引用:**

> ***Hi 即使在做了* `*proxy_server.stop()*` *和* `*proxy.close()*` *browsermob 代理之后，还是离开了在 Mac 后台运行的 java 进程。***

**作为过程组存在未合并的部分修复[运行和压井。](https://github.com/AutomatedTester/browsermob-proxy-py/pull/71)**

**在这里，你可以找到一些关于如何在 Windows 上关闭 BMP Dameon 的指南。**

# **这是怎么回事？**

**根本原因如下。当调用 BMP 守护进程的`start()`方法时，它使用*子进程。Popen* 带有命令`sh browsermob-proxy -port 8080`的某种变体。如上所述，在 Windows Python process start 的 cmd 上的*一节中，执行启动 Java 进程的 bat 文件。***

***当你在 *bmp_daemon* 上调用`stop()`函数时，它成功地杀死了`conhost.exe`进程。在 Windows 上(在 Mac 上按照 [#76](https://github.com/AutomatedTester/browsermob-proxy-py/issues/76) )操作系统不会自动杀死宏进程(我不知道在 Linux 上发生了什么，但我怀疑它能起作用)，也就是说`stop()`函数杀死了`conhost.exe`进程，但在 Windows(和 Mac)上 Java 进程存活了下来。***

**[建议修复](https://github.com/AutomatedTester/browsermob-proxy-py/pull/71/commits/bf03dd027adc75f298008ee00dee941b295dcf7c)简单来说如下:**

```
**self.log_file = open(os.path.join(log_path, log_file), 'w')self.process = subprocess.Popen(self.command,                                                   **preexec_fn=os.setsid, **                                                  stdout=self.log_file,                                                   stderr=subprocess.STDOUT)group_pid = os.getpgid(self.process.pid)
self.process.kill()                               self.process.wait()
os.killpg(group_pid, signal.SIGINT)**
```

**据称，这一修复工作在 Mac 上。**

**在 Windows 上，此修复程序具有以下形式:**

```
**self.log_file = open(os.path.join(log_path, log_file), 'w')self.process = subprocess.Popen(self.command,                                                   **creationflags=subprocess.CREATE_NEW_PROCESS_GROUP,  **                                                 stdout=self.log_file,                                                   stderr=subprocess.STDOUT)group_pid = self.process.pid
self.process.kill()                               self.process.wait()
os.killpg(group_pid, signal.SIGINT)**
```

**这在 Windows 上不起作用。**

**基本上，上面的代码试图做的是围绕`browsermob-proxy -port 8080`创建“进程组”，这样所有的子进程将被分组到一个实体中，这个实体可以使用 OS 系统调用来终止。**

**这里[https://github . com/automated tester/browser mob-proxy-py/issues/8 # issue comment-679150656](https://github.com/AutomatedTester/browsermob-proxy-py/issues/8#issuecomment-679150656)是同一思路的另一种实现。我的解决方案是另一个实现。**

**我不修改现有的代码，而是使用相同的包装功能。基本上，我在做以下事情:**

1.  **我使用`psutil`来获取作为执行`subprocess.Popen(self.command..)`的一部分而创建的所有子进程。在内部，每个这样的进程都有 *(PID* 和它的*创建时间)。***
2.  **我叫`bmp_daemon.stop()`。这很重要，因为它有下面的调用。该呼叫确保`self.log_file`将被正确关闭。**
3.  **现在，我从 p.1 开始迭代子进程。调用
    `import psutil
    from contextlib import suppress`
    `with suppress(psutil.NoSuchProcess):
    child.send_signal(signal.SIGTERM)` **注意:**这个命令只能终止我们之前在 p.1 中缓存的进程。如果进程已经完成，我们将得到我们刚刚取消的`NoSuchProcess`(`send_signal()`有装饰者`@_assert_pid_not_reused` 负责不终止新进程的重用 id；在内部，它通过 PID 获取进程，并检查它是否具有相同的*创建时间*，如果不相同`NoSuchProcess` 被引发)。**

**总之，我们正在获取在`bmp_daemon.start()`中打开的所有子进程(孙进程和所有祖先进程)id。我们调用`bmp_daemon.stop()`，然后杀死所有祖先的进程。**

**如果进程同时终止，我们忽略 If。**

**如果流程 id 被重用，我们也不做任何事情。**

# **BrowserDataDir，BMPProxy，SeleniumWebDriver，截图**

**代码:**

***dd* 描述如下:**

****注:**关于如何将 yml 文件转换成 dict *dd* 参见上面*的“许多函数以字典为参数”*一节。**

**解释是:**

**`***BrowserDataDir***` *—* 该上下文管理器可用于重用用户数据目录。它将文件`template` (在我们的例子中为*' path/to/chrome _ data _ dir . zip '*)解压到`work_dir`(在我们的例子中为' *logs'* )。**

**它返回包含从模板中提取的内容的目录(在我们的例子中是`browser_data_dir`)。**

**在从上下文管理器内的代码块退出时，`work_dir` 被移除。**

**稍后我们将使用`browser_data_dir`来填充作为 kwargs 传递给`SeleniumWebDriver`的`d[‘web_driver’][‘arguments’]`。**

****注意:**在配置 yml 中，我们有以下行`‘--user-data-dir={data_dir}’`。调用`_arg_format()`将有效地将`{data_dir}`替换为`browser_data_dir`。
如果你不需要将`--user-data-dir`传递给 Selenium Web 驱动(比如`chromedriver.exe`)，那么
你应该移除`*BrowserDataDir,*`移除行`‘--user-data-dir={data_dir}’` ，不要填充`d[‘web_driver’][‘arguments’]` (kwargs to `SeleniumWebDriver`)。**

**`***BMPPRoxy***` *—* 该上下文管理器设计用于在新端口上创建`BMP Proxy`。假设`BMPDaemon`已经启动。它不需要`BMPDaemon`对象，只需要它的一些参数。**

**默认情况下，它假设`BMPDaemon`正在本地主机上运行。默认情况下，它还假设运行在端口 8080 上，您可能希望通过提供`d[‘browsermob’][‘daemon][‘init][‘options’][‘port’]`来覆盖该值。建议提供您已经提供给`BMPDaemon`的相同字典。**

**如果您还记得上面的代码片段，它使用`BMPDaemon`上的`create_proxy()`方法来创建`*BMPPRoxy*` ，并在内部重用一些参数。我的实现通过模仿`BMPDaemon` 的行为绕过了`create_proxy()`方法的使用。**

**这个上下文管理器返回 BMP 代理。**

**在退出上下文管理器中的代码块时，它关闭了 BMP 代理。**

**`**SeleniumWebDriver**` —该上下文管理器旨在创建 Web 驱动程序。它假设 BMPDaemon 已经启动。想用可以通过`*BMPPRoxy*` 。它不需要 BMPDaemon 对象。它返回 Web 驱动程序。在退出上下文管理器内的代码块时，关闭 Web 驱动程序，详见`closeSeleniumWebDriver()`。**

**一些**基础架构**描述:Selenium 是框架。它的主要组件之一是 Web 驱动程序。对于每一个主流浏览器，比如 Google Chrome，Mozilla Firefox，都有专门的 Web 驱动类型。这样，每种 Web 驱动程序类型都“知道”相应浏览器的内部结构。Web 驱动程序的主要目的是提供与浏览器无关的 API。在内部，每个 Web 驱动程序“知道”将这种浏览器不可知的 API 调用转换为特定的浏览器 API 调用。**

****注意:** Web 驱动由 Python 包装器和一些可执行文件组成(比如`chromedriver.exe`)。术语“网络驱动程序”(容易混淆)指的是两者。**

**硒-3 幸运的一些功能，例如，网络监控。有许多 Selenium *扩展*来提供额外的功能。这个*扩展*中的一个就是`*BMPPRoxy*`T12。它的主机&端口可以作为 Web 驱动选项的参数传递；这样浏览器和互联网之间的所有网络通信都将通过`*BMPPRoxy*` *。* **注意:** Selenum-4 会内置这个功能。**

****注意:**如何创建适用于*任何*网络驱动的**通用上下文管理器有些复杂。例如，谷歌 Chrome 和 Mozilla Firefox 有不同的选项类。我的实现灵感来自于[*https://github . com/clemfromspace/scrapy-selenium/blob/develop/scrapy _ selenium/middleware . py*](https://github.com/clemfromspace/scrapy-selenium/blob/develop/scrapy_selenium/middlewares.py)****

**在上面的代码示例中，我传递了*browsermoproxy*(它的主机&端口将作为 Web 驱动程序选项的参数传递)，此外，我还传递了 *web_driver* 。它包含以下条目:**

***'名称':'铬'***

**它用于确定特定的(浏览器的)Web 驱动程序。在这种情况下它将是*' selenium . web driver .****chrome****。webdriver。***

***' log _ file ':' path/to/logs/chrome driver . log '***

**来自 Selenium 的 Web 驱动组件的所有日志都将被重定向到这个 *log_file。***

****注意:**这在关闭 Web 驱动程序时会产生一些问题。它们在下面的`closeSeleniumWebDriver()`中处理。**

***“路径”:“资源/chrome driver . exe”***

**Web 驱动程序的可执行文件的路径。Python 包装器需要它来调用它。这是实际浏览器控件所在的位置。**

**参数' =[' - headless '，'- window-size=1920，1080 '，'- ignore-certificate-errors '，'-disable-useAutomationExtension '，'-user-data-dir = logs/chrome _ DCM _ data _ dirvwg1 nmbv ']**

**实验 _ 选项' *:* '排除开关':['启用-记录'，'启用-自动化']**

**我们一个一个来。**

****无头模式。****

**无头浏览器是没有图形用户界面的浏览器。在无头模式下运行浏览器意味着它可以在不需要专用显示器或图形的情况下运行。它之所以被称为 headless，是因为它运行时没有图形用户界面(GUI)。程序的 GUI 被称为头部。**

**大约在 2017 年之前，所有主流浏览器都不支持**无头模式**。所以，如果你想用 Selenium 运行 **headless** 你应该使用[另一个浏览器](https://www.selenium.dev/documentation/en/getting_started_with_webdriver/browsers/#specialized-browsers)，比如 [HtmlUnit 或者 PhantomJS](https://www.selenium.dev/documentation/en/webdriver/driver_requirements/#mock-browsers) 。**

**[无头 Chrome 在 Chrome 59](https://developers.google.com/web/updates/2017/04/headless-chrome) 中发货。另请参见此处的。**

**[在 Firefox 中使用无头模式](https://hacks.mozilla.org/2017/12/using-headless-mode-in-firefox/#usage)。参见[这里的](https://askinglot.com/open-detail/113464)。**

**因此，大约从 2017 年开始，你可以在**无头模式下使用你最喜欢的浏览器(它也存在于这里没有提到的另一个浏览器中)。****

**我个人在`dev`用的是**完整版浏览器**，在`prod`用的是**无头模式**。我正在为我的配置文件使用不同的*概要文件*来实现这一点。有关这方面的更多信息，请参见[我的专业初始化应用配置模块](/analytics-vidhya/my-major-init-app-conf-module-1a5d9fb3998c)**

****窗口大小****

**[1366x768](https://github.com/mozilla/geckodriver/issues/1354) 是 Mozilla Firefox 中无头模式的默认分辨率。在无头模式下，谷歌 Chrome 浏览器启动于 [800x600](https://itnext.io/how-to-run-a-headless-chrome-browser-in-selenium-webdriver-c5521bc12bf0) 。许多网站“看起来很丑”,在这样的分辨率下几乎不可用——比如，你不能点击按钮，所以你应该覆盖它的默认设置。**

****忽略证书错误****

**在开始 *https* 通信之前，网站会向 browser 提供一个证书来标识自己。有些网站使用不被浏览器信任的*自签名证书*(也可能是因为其他原因，但这是最常见的原因)*、*，因此您会得到一个错误页面，显示消息“[您的连接不安全”](https://stackoverflow.com/questions/24507078/how-to-deal-with-certificates-using-selenium)。**

**如果你使用的是 Google Chrome，在 argument 的选项中传递`--ignore-certificate-errors`是解决这个问题最简单的方法。**

**有关替代方案或其他浏览器的解决方案，请参见[http://all selenium . info/self signed-certificates-python-selenium/](http://allselenium.info/selfsigned-certificates-python-selenium/)**

****注意:**目前不支持 *desired_capabilities* 。参见[https://github.com/alex-ber/selenium-support/issues/1](https://github.com/alex-ber/selenium-support/issues/1)**

****禁用-使用自动扩展****

**这是谷歌浏览器特有的功能。**

**这应该和 will 连用**

```
**options = webdriver.ChromeOptions()
options.add_experimental_option('excludeSwitches', ['enable-logging', 'enable-automation'])**
```

**[https://stack overflow . com/questions/47392423/python-selenium-dev tools-listening-on-ws-127-0-0-1](https://stackoverflow.com/questions/47392423/python-selenium-devtools-listening-on-ws-127-0-0-1)**

**[https://stack overflow . com/questions/46423361/chrome-devmode-turn-on-in-selenium](https://stackoverflow.com/questions/46423361/chrome-devmode-suddenly-turning-on-in-selenium)**

**这 3 个选项将删除弹出的`DevTools`消息，并阻止进入谷歌 Chromes 的 devmode。**

****用户数据目录****

**这是谷歌浏览器特有的功能。有关 Mozilla Firefox 的替代方案，请参见[https://stackoverflow.com/a/55636113/1137529](https://stackoverflow.com/a/55636113/1137529)或[https://Firefox-source-docs . Mozilla . org/testing/geckodriver/crash reports . html](https://firefox-source-docs.mozilla.org/testing/geckodriver/CrashReports.html)**

**这是完全可选的功能。最好和`BrowserDataDir`(见上)一起用。**

**如果你想让你的浏览器使用一些预定义的用户数据目录(Mozilla Firefox 中的 profile)。**

**比如在 Windows 上可以创建快捷方式:*" C:\ Program Files(x86)\ Google \ Chrome \ Application \ Chrome . exe "-user-data-dir = C:\ tmp \ Chrome _ profile。*点击上面的创建所有内部文件夹，然后将*' C:\ tmp \ Chrome _ profile '*作为`--user-data-dir`传递给 Google Chrome option 的参数。**

# **关闭 Selenium 的 Web 驱动程序**

**如果你使用`SeleniumWebDriver`作为上下文管理器，它会担心关闭网络驱动。如果想自己做，可以调用`closeSeleniumWebDriver()` 函数。**

**你可能想知道为什么打电话到`web_driver.quit()`还不够。**注意**(注意！):你不要把这个电话和`web_driver.close()`搞混了。最后一次调用只会关闭标签页，而不会关闭浏览器本身。**

**如果您在阅读上面几行时有似曾相识的感觉，您可能会想起，在`closeBmpDaemon()`函数中也发生了类似的事情。**

**我不会重复我自己，实现是非常接近的。**

****但是为什么** `**web_driver.quit()**` **还不够呢？****

**我注意到，有时，当异常出现时，但并不总是如此，谷歌 Chrome 浏览器和/或`chromedriver.exe`仍然驻留在内存中。**

**当我添加了相同的逻辑来关闭在 Web 驱动程序初始化中打开的所有祖先进程时，这种情况再也不会发生了。**

# **`Screenshot`**

**它被设计用作上下文管理器。**

**如果想要 API 进行简单的函数调用，请使用`save_screenshot()`。**

**你可能想用这个上下文管理器来保护你的代码。要求您首先实例化`web_driver`。如果在上下文管理器内部的代码块中将引发异常，那么将采取一些额外的操作。**

**如果`logger` 不为 None，将向其发出警告记录器消息。**

**如果我们有一个带有`screen`的`WebDriverException`实例，它将被用来生成截图的 png 文件。**

**如果`screen`没有或者我们没有`WebDriverException`的实例，我们会主动截图。请注意，此尝试可能会失败。**

**在上面的代码片段中:**

**`web_driver`是必填字段。`Action`只是异常发生的指示。`screenshots_dir`是保存截图的目录。`logger`已通过，有`warning`消息发出。**

# **`Save screenshot`**

**`save_screenshot()`是常规函数 API。如果您想要一个上下文管理器，请使用`Screenshot`。**

**也许，你有一些`try-finally`块，当你从`web_driver`捕捉到一个异常时，你想要得到截图，以便更好地理解哪里出错了。就我个人而言，我更喜欢使用`Screenshot`，但是在一些复杂的场景中，你可能想要更好的控制。**

****注:**您可以将*DD[' browser _ download _ folder '****]***定义如下**

```
**dd['files']['browser_download_folder'] = str(Path(Path.home(), 'Downloads'))**
```

# **在 Google Chrome 中启用下载**

**`enable_chrome_download()`是谷歌 Chrome 特有的功能。**

**在谷歌浏览器中，无头模式下的下载是默认禁用的。这是一个“特征”，为了安全。如果您想启用下载，您可以使用此功能。详情请见[https://stack overflow . com/questions/45631715/downloading-with-chrome-headless-and-selenium](https://stackoverflow.com/questions/45631715/downloading-with-chrome-headless-and-selenium)。**

# **设定新 HAR**

**`set_new_har()`是带有`capture*`参数的`bmp_proxy.new_har()`的方便包装器。它用于获取网络传输。例如，如果你想得到响应体。详见[https://medium . com/@ jiurdqe/how-to-get-JSON-response-body-with-selenium-amd-browser mob-proxy-71f 10335 c66](/@jiurdqe/how-to-get-json-response-body-with-selenium-amd-browsermob-proxy-71f10335c66)。**

**此呼叫相当于在页面检查器的“网络”选项卡上单击“开始录音”。**

**在上面的示例代码中，我们调用了`set_new_har(bmp_proxy, har_name)` ，其中`bmp_proxy`是上面创建的对象，`har_name`是 str。该调用完成后，从浏览器到站点的所有网络通信都记录在`BMP Proxy`中。**

**如果您想要检索它，您可以使用以下模板:**

****注**:**

1.  **在第 4 行中，我们等待(5 毫秒)网络流量停止(超时 70 毫秒)。需要确保 BMP 代理已经记录了所有网络流量(到目前为止)。**
2.  **第 9 行中`reversed`的用法。引用的链接没有这个。如果您对同一个 URL 有多个调用，您应该查看最后的结果，因此您应该颠倒日志条目的顺序。**

# **等待加载一些基本页面元素**

**`wait_page_loaded()`是辅助函数，保证页面的一些基本元素，比如标题都被加载。**

**使用示例:**

```
**from alexber.seleniumsupport import wait_page_loaded
wait = WebDriverWait(web_driver, timeout=70, poll_frequency=1)
wait_page_loaded(wait)**
```

# **点击`WebElement`**

**`click_sync()` -有时在`WebElement`上调用`click()`会引发一些奇怪的异常。最佳实践是使用`wait.until(EC.element_to_be_clickable((By.XPATH, **‘**xpath**’**)))`。这是对`WebElement.click()`
( `WebElement`通常是按钮)进行同步调用(通过使用 JavaScript)的“肮脏”解决方案。详见[https://stackoverflow.com/a/58378714/1137529](https://stackoverflow.com/a/58378714/1137529)。**

**使用示例:**

```
**from alexber.seleniumsupport import click_sync
wait = WebDriverWait(web_driver, timeout=70, poll_frequency=1)click_sync(web_driver,
                 wait.until(EC.element_to_be_clickable(
                    (By.XPATH, **'**xpath**'**))))**
```

# **等待谷歌浏览器完成下载**

**`wait_chrome_file_finished_downloades()`是谷歌 Chrome 特有的功能。
它直接与文件系统一起工作。你应该事先知道*文件名*。它依赖于 Google Chrome 的以下内部机制:**

*   **谷歌 Chrome 下载文件时，有扩展名*”。crdownload"* 。当下载完成后，谷歌浏览器重命名文件删除这个扩展名。**

**这个功能**不依赖**谷歌 Chrome 的下载状态。这种设计选择有两个主要原因:**

*   **我们可以依靠谷歌浏览器下载页面的一些实现细节。他们往往会毫无征兆地改变，所以我选择不改变。参见[https://stack overflow . com/questions/48263317/selenium-python-waiting-a-download-process-to-complete-using-chrome-web/48267887 # 48267887](https://stackoverflow.com/questions/48263317/selenium-python-waiting-for-a-download-process-to-complete-using-chrome-web/48267887#48267887)**
*   **在 Windows 上，我们可以看到文件系统的错误状态。引用 Python 文档:**

> *****注意:*** *在基于 Unix 的系统上，* `[*scandir()*](https://docs.python.org/3/library/os.html#os.scandir)` *使用系统的* `[*opendir()*](http://pubs.opengroup.org/onlinepubs/009695399/functions/opendir.html)` *和* `[*readdir()*](http://pubs.opengroup.org/onlinepubs/009695399/functions/readdir_r.html)` *函数。在 Windows 上，它使用 Win32* `[*FindFirstFileW*](https://msdn.microsoft.com/en-us/library/windows/desktop/aa364418(v=vs.85).aspx)` *和* `[*FindNextFileW*](https://msdn.microsoft.com/en-us/library/windows/desktop/aa364428(v=vs.85).aspx)`*函数。***

**[https://docs.python.org/3/library/os.html#os.scandir](https://docs.python.org/3/library/os.html#os.scandir)**

**引用自`[*FindFirstFileW*](https://msdn.microsoft.com/en-us/library/windows/desktop/aa364418(v=vs.85).aspx)`和`[FindNextFileW](https://msdn.microsoft.com/en-us/library/windows/desktop/aa364428(v=vs.85).aspx)*:*`**

> ***要将此操作作为事务处理操作执行，请使用*[*FindFirstFileTransacted*](https://docs.microsoft.com/en-us/windows/desktop/api/winbase/nf-winbase-findfirstfiletransacteda)*函数。
> …
> 如果存在绑定到文件枚举句柄的事务，则返回的文件受事务隔离规则的约束。
> …* ***注意:*** *在极少数情况下或在负载较重的系统上，NTFS 文件系统上的文件属性信息在调用此函数时可能不是最新的。为了确保获得当前 NTFS 文件系统的文件属性，调用*[*GetFileInformationByHandle*](https://docs.microsoft.com/en-us/windows/desktop/api/fileapi/nf-fileapi-getfileinformationbyhandle)*函数。***

**[https://msdn . Microsoft . com/en-us/library/windows/desktop/aa 364418(v = vs . 85)。aspx](https://msdn.microsoft.com/en-us/library/windows/desktop/aa364418(v=vs.85).aspx)**

**[https://msdn . Microsoft . com/en-us/library/windows/desktop/aa 364428(v = vs . 85)。aspx](https://msdn.microsoft.com/en-us/library/windows/desktop/aa364428(v=vs.85).aspx)**

**那些“罕见病例”并不罕见。我经常遇到它们:谷歌 Chrome 从另一个进程下载文件，它有*”。crdownload* 扩展名。我正在用 Python 代码检查带有*的文件。crdownload"* 扩展存在并得到结果，它没有找到，但它显然仍然下载。我认为需要一些时间才能看到对 filesysem(更高级的文件属性)的更改。为了减轻这个问题，我增加了简单的睡眠重试机制。在我咨询文件系统之前，我会休眠一段时间，以确保对文件系统所做的任何更改对 Python 进程都是可见的。**

****注**:如果文件非常大(超过 200MB)，您可能需要增加重试次数。**

# **等待显示**

**有时，我们想让 Selenium Web 驱动程序等到元素样式属性改变后再运行。这对于动态加载的材料很有用。例如，我们希望等待显示样式变为*【无】*(或变为`*“inline-block"*` 或其他值)。参见[https://stack overflow . com/questions/34915421/make-selenium-driver-wait-until-elements-style-attribute-has-changed](https://stackoverflow.com/questions/34915421/make-selenium-driver-wait-until-elements-style-attribute-has-changed)**

**使用示例:**

```
**from alexber.seleniumsupport import wait_for_display
wait = WebDriverWait(web_driver, timeout=70, poll_frequency=1)wait.until(wait_for_display((By.XPATH, **'**xpath**'**)))**
```

# **如需进一步阅读:**

**定位元素
https://www . selenium . dev/documentation/en/getting _ started _ with _ web driver/locating _ elements/**

**Chrome driver—Chrome
[https://sites.google.com/a/chromium.org/chromedriver/](https://sites.google.com/a/chromium.org/chromedriver/)的 WebDriver**

**浏览器操作
[https://www . selenium . dev/documentation/en/web driver/browser _ Manipulation/# browser-navigation](https://www.selenium.dev/documentation/en/webdriver/browser_manipulation/#browser-navigation)**

**比赛条件按钮(等待)
[https://www.selenium.dev/documentation/en/webdriver/waits/](https://www.selenium.dev/documentation/en/webdriver/waits/)**

**从元素(Web 元素)
[中查找元素 https://www . selenium . dev/documentation/en/Web driver/Web _ Element/](https://www.selenium.dev/documentation/en/webdriver/web_element/)**

**使用选择元素
[https://www . selenium . dev/documentation/en/support _ packages/working _ with _ select _ elements/](https://www.selenium.dev/documentation/en/support_packages/working_with_select_elements/)**

**WebDriver API 提供了为浏览器设置代理的能力，
并且有许多代理将通过编程允许您操作
发送到 web 服务器
[和从 web 服务器接收的请求的内容 https://www . selenium . dev/documentation/en/worst _ practices/http _ response _ codes/](https://www.selenium.dev/documentation/en/worst_practices/http_response_codes/)**

**Selenium 4 Alpha 现已上市 2020 年 11 月 17 日
https://testguild.com/selenium-4/**

**我们使用的是 Chrome 驱动 XXX 版本，请看 Chrome 驱动参数
[https://Peter . sh/experiments/chromium-command-line-switches/](https://peter.sh/experiments/chromium-command-line-switches/)**

**问题 696481:无头模式不保存文件下载
https://bugs.chromium.org/p/chromium/issues/detail?[id = 696481](https://bugs.chromium.org/p/chromium/issues/detail?id=696481)
[https://stack overflow . com/questions/45631715/downloading-with-chrome-headless-and-selenium](https://stackoverflow.com/questions/45631715/downloading-with-chrome-headless-and-selenium)**

**browsermob-proxy GitHub
设置代理，提取 SAML 断言
[https://medium . com/@ arkadyt/Setting-Up-programmable-AWS-access-for-MFA-protected-federated-identities-3be 22 bcecf 4b](/@arkadyt/setting-up-programmatic-aws-access-for-mfa-protected-federated-identities-3be22bcecf4b)
[https://github.com/lightbody/browsermob-proxy](https://github.com/lightbody/browsermob-proxy)(可能是漏读！！！)
[https://medium . com/@ jiurdqe/how-to-get-JSON-response-body-with-selenium-amd-browser mob-proxy-71f 10335 c66](/@jiurdqe/how-to-get-json-response-body-with-selenium-amd-browsermob-proxy-71f10335c66)**

**Google Chrome/Mozilla Firefox 推荐的扩展:**

**XPath 助手**

**检查员**