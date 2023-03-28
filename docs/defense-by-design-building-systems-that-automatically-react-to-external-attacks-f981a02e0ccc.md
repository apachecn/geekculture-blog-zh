# 设计防御:构建自动应对外部攻击的系统

> 原文：<https://medium.com/geekculture/defense-by-design-building-systems-that-automatically-react-to-external-attacks-f981a02e0ccc?source=collection_archive---------26----------------------->

![](img/f3e3821ad50001a5f046a5aedf6fef6d.png)

# 互联网是一个充满敌意的地方

Web 应用程序安全性是一个大话题，也是系统管理员的一个持续挑战，当您设置任何服务器并将其连接到互联网时，该服务器就会成为黑客、机器人和恶意用户请求的乐园。

# 监控与自动防御

监控服务器是一项艰巨的任务，如果我们建立一个自动防御系统，对不同的攻击和威胁自动做出反应，而不是查看不同服务的日志并对不同的攻击做出反应，这不是更好吗？

在本文中，我将解释如何使用一些现有的工具来构建一个设计安全的系统。因此，大多数攻击都会被系统自动拒绝。

# 开始之前

首先，确保在首次设置服务器时遵循标准的安全建议和最佳实践，这包括禁用 root 访问和 ssh 密码登录，启用基本防火墙规则来阻止不必要的端口和服务等..

下面是一个很好的入门参考:

[](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04) [## 使用 Ubuntu 20.04 的初始服务器设置|数字海洋

### 当你第一次创建一个新的 Ubuntu 20.04 服务器时，你应该执行一些重要的配置步骤作为…

www.digitalocean.com](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-20-04) 

# 使用 Fail2ban 进行自动防御

[**Fail2ban**](https://github.com/fail2ban/fail2ban) 是一个优秀的开源**入侵检测**/防御系统(IDS/IPS)，我们将依靠这个伟大的工具来构建我们的防御。

您可以通过以下命令在 Ubuntu 上轻松安装这个库

```
$ sudo apt install fail2ban
```

**fail 2 ban 如何工作**

fail2ban 背后的基本思想是监视公共服务的日志以发现特定的模式(这是通过 Fail2ban **过滤器**完成的)。

当发现特定模式时，可以执行特定的触发器(这是通过 Fail2ban **操作**完成的)，默认操作是通过修改`iptables`防火墙规则来禁止违规主机/IP 地址，如果您使用 CloudFlare CDN 来进一步优化和保护您的 web 应用程序，则可以使用自定义操作来禁止 CloudFlare 网络本身上的违规 IP。

`jail.conf`是包含许多服务的 jail 配置的主配置文件。这个文件可以在更新中被覆盖，所以鼓励用户将这个文件复制到一个`jail.local`文件中，并在那里进行调整。

# 拦截 99%的恶意机器人和暴力攻击

感谢我们上面讨论的强大功能，阻止恶意请求从未如此简单。

下面我列出了一些示例过滤器，您可以使用它们来阻止常见类型的攻击。

**Nginx Bot 搜索过滤器** 此过滤器随 Fail2ban 一起提供，将检测已知 Bot 使用的常见模式，您可以在`filters.d`目录中找到此过滤器。

```
# Fail2Ban filter to match web requests for selected URLs that don't exist#[INCLUDES]# Load regexes for filteringbefore = botsearch-common.conf[Definition]failregex = ^<HOST> \- \S+ \[\] \"(GET|POST|HEAD) \/<block> \S+\" 404 .+$^ \[error\] \d+#\d+: \*\d+ (\S+ )?\"\S+\" (failed|is not found) \(2\: No such file or directory\), client\: <HOST>\, server\: \S*\, request: \"(GET|POST|HEAD) \/<block> \S+\"\, .*?$ignoreregex =datepattern = {^LN-BEG}%%ExY(?P<_sep>[-/.])%%m(?P=_sep)%%d[T ]%%H:%%M:%%S(?:[.,]%%f)?(?:\s*%%z)?^[^\[]*\[({DATE}){^LN-BEG}
```

**Nginx 字节攻击**

在 filters.d 目录中添加一个名为 nginx-x00.conf 的文件，结合 nginx access.log 使用该文件来检测字节攻击。

```
[Definition]
failregex = ^<HOST> .* ".*\\x.*" .*$
```

**Nginx 4xx 扫描检测**

几乎所有扫描/暴力工具都会尝试攻击应用程序的随机端点来查找漏洞，该过滤器将帮助您对特定的 4xx 失败尝试序列做出反应，并相应地触发禁止操作。

```
[Definition]failregex = ^<HOST>.*"(GET|POST).*" (404|444|403|400) .*$ignoreregex =
```

**检测直接访问脚本文件的企图**

如今，大多数 web 框架都避免使用直接请求脚本文件的 URL。php/。py/。js 等..)，因此，通过以下过滤器检测利用脚本文件的企图将非常方便:

```
[Definition]failregex = ^<HOST> -.*GET.*(\.php|\.py|\.asp|\.exe|\.pl|\.cgi|\scgi)ignoreregex =
```

**检测并阻止您自己的自定义 URL 模式**

如果您了解自己的堆栈，那么阻止任何试图访问堆栈中不存在的特定 URL 模式的客户端会很有用。例如，Django/Ruby 应用程序可以安全地阻止试图访问普通 PHP/WordPress URL 的客户端，你也可以阻止那些试图访问敏感配置/点文件的客户端。下面是一个过滤器示例:

```
[Definition]failregex = ^<HOST> -.*GET.*(phpmyadmin|wp-login|\.env*|\.git*)ignoreregex =
```

**检测并阻止 DDOS 攻击**

除了搜索特定的模式，我们还可以将 below 过滤器与 Nginx 速率限制模块结合使用，以确定大量的请求并触发自动阻塞。

```
# Fail2Ban configuration file
#
# supports: ngx_http_limit_req_module module

[Definition]

failregex = limiting requests, excess:.* by zone.*client: <HOST>

# Option: ignoreregex
# Notes.: regex to ignore. If this regex matches, the line is ignored.
# Values: TEXT
#
ignoreregex =
```

# 结论

正如我们在上面的例子中看到的，Fail2ban 是一个强大的框架，可以用来构建一个非常强大的自动防御触发器，可以定制配置以适应您希望保护的任何服务。