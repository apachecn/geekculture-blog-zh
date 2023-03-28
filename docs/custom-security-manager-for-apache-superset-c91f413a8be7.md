# Apache 超集的自定义安全管理器

> 原文：<https://medium.com/geekculture/custom-security-manager-for-apache-superset-c91f413a8be7?source=collection_archive---------5----------------------->

![](img/0f284ad59365652a27fa617e605f7906.png)

Photo by [Tabrez Syed](https://unsplash.com/@tabrez_syed?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

2021 年 4 月，Apache 超集社区[发布了版本 1.1.0](https://github.com/apache/superset/tree/master/RELEASING/release-notes-1-1) ，它为我们带来了更强大的可视化、更简单的安装和许多错误修复。超集在商业智能(BI)工具领域变得越来越流行。越来越多的公司决定使用 Superset 作为他们的主要 BI 系统，并希望了解其安全架构。*认证是安全子系统中最重要的部分之一。*那么，默认情况下，超集中有哪些选项？

超集建立在 [Flask App builder](https://github.com/dpgaspar/Flask-AppBuilder) (FAB)之上。它有许多内置的认证方法。最好使用其中一个，因为它很可能会满足您的需求，并且由社区维护良好:

*   **数据库** —从数据库中查询要匹配的用户名和密码样式。密码在数据库中保持散列。
*   **开放 ID** —使用用户的电子邮件字段在 Gmail、Yahoo 等上进行认证。
*   **LDAP** —针对 LDAP 服务器的身份验证，如 Microsoft Active Directory。
*   **REMOTE_USER** —当服务器(Apache、Nginx)被配置为使用 kerberos 时，web 服务器负责对用户进行身份验证，这对于内部网站点非常有用。
*   **OAUTH** —使用 OAUTH (v1 或 v2)进行认证。

如您所见，默认情况下，您可以使用许多选项进行安装。但是许多并不意味着全部，对吗？在本文中，我们将介绍创建 Apache 超集*自定义安全管理器*所需的接下来三个简单步骤，这将有助于理解如果默认超集(FAB)选项不能满足我们的需求，该怎么办。

首先，创建一个新文件 **my_security_manager.py，**并把它放在你的 **PYTHONPATH** 目录下。未来的超集定制将有一个基础。将这些行添加到文件中:

```
from superset.security import SupersetSecurityManager class MySecurityManager(SupersetSecurityManager): def __init__(self, appbuilder):
        super(MySecurityManager, self).__init__(appbuilder)
```

其次，您应该让 Superset 知道您想要使用全新的安全管理器。为此，请将这些行添加到超集配置文件(superset_config.py)中:

```
from my_security_manager import MySecurityManager
CUSTOM_SECURITY_MANAGER = MySecurityManager
```

第三，将这一行放到您的 **Dockerfile** 中，以便在运行之前将您的自定义设置和新的安全管理器复制到容器中:

```
COPY my_security_manager.py /app/pythonpath
```

就是这样！重新构建你的 Docker 容器，并转到 Superset 来检查它是否正常工作。

现在，您可以以任何方式扩展您的安全管理器。太神奇了！假设您想要为登录页面创建自己的视图。为此，您可以覆盖 MySecurityManager 类的 **authdbview** 属性。你可以以[这个原始视图代码](https://github.com/dpgaspar/Flask-AppBuilder/blob/0525e549ef7170f5623af4244f61991ad19bc215/flask_appbuilder/security/views.py#L483)为例，对其进行扩展。或者，您可能想要创建不同的认证逻辑，例如多域 LDAP 或 SMS 网关。您可以覆盖 MySecurityManager 类的 **auth_user_db()** 方法，或者创建一个新方法。对于您自己的身份验证视图，不要忘记在您自己的视图实现中使用它。这里是[方法](https://github.com/dpgaspar/Flask-AppBuilder/blob/0525e549ef7170f5623af4244f61991ad19bc215/flask_appbuilder/security/manager.py#L821)的原代码，供参考。

希望你喜欢阅读这篇文章。请关注我的[媒体](/@agordienko)、 [GitHub](https://github.com/aleksandrgordienko) 、 [Twitter](https://twitter.com/data_diving) 、 [LinkedIn](https://www.linkedin.com/in/aleksandrgordienko/?lipi=urn%3Ali%3Apage%3Ad_flagship3_feed%3BBS0l2fduQoWolGSffxn%2F1w%3D%3D) 。

请阅读前一篇文章中[关于在 Docker 容器中运行超集的附加信息。](/geekculture/run-apache-superset-locally-in-10-minutes-30bc70ed808c)