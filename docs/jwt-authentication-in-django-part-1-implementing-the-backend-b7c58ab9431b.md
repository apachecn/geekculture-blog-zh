# Django 中的 JWT 认证，第 1 部分:实现后端

> 原文：<https://medium.com/geekculture/jwt-authentication-in-django-part-1-implementing-the-backend-b7c58ab9431b?source=collection_archive---------0----------------------->

![](img/e4420433217922de165a7fb91d92587a.png)

*本文介绍了使用 Django 后端和独立前端(如 React 或 Vue)实现 JWT 认证的过程。由于这个题目是比较中级的水平，所以假设对以下有一点了解:*

*   *姜戈*
*   *Django Rest 框架*

*如果你想直接跳到第 2 部分(前端的所有东西)，点击* [*这里*](https://johnckealy.medium.com/jwt-authentication-in-django-part-2-implementing-the-frontend-7ea3d6e16bf4) *。*

# JWT 认证，一个有争议的话题？

身份验证(以及它的兄弟，授权)是现代网络的一个重要组成部分。这就是为什么我如此惊讶地发现围绕它仍然存在这样的争议。如果你深入研究，你会发现博客和文章似乎相互矛盾，JWT 经常在 sh*t 名单上。

我写这篇文章的原因是为了展示我在解耦的 Django 项目中尝试设置身份验证时所学到的东西，希望能为您节省一些痛苦。我想要完成的目标如下:

*   在一个新的或已存在的项目中设置认证/授权，由一个独立的 Django Rest 框架后端和一个 Vue.js 前端提供支持。
*   在顶点域(例如`example.com`)托管前端，在子域(例如`api.example.com`)托管后端
*   使用 JWT (JSON Web 令牌)作为身份验证方法
*   不要把我的笔记本电脑扔出窗外

## 那么，为什么 JWT 认证会有争议呢？

一些开发者会简单地告诉你不要使用 JWT，尽管它很受欢迎。我认为这种对 JWT 的憎恨很大程度上源于这样一个事实，即它是无状态的，这意味着如果攻击者能够获得您的访问令牌，无论它来自哪里，服务器都会信任它。跨站点脚本(XSS)是这里的一个关键漏洞，在使用会话身份验证时，这不是一个令人担心的问题。另一方面，开发人员在没有真正需要的情况下实现 JWT 并不少见，会话认证是一种安全可靠的替代方法。

许多解释 JWT 的博客和教程会漫不经心地告诉读者将访问令牌存储在本地存储或 cookies 中。因为 Javascript 可以访问这些，所以可能会发生不好的事情。

无论如何，本文的重点不是讨论 JWT，它假设您已经决定 JWT 是您的用例的最佳选择。本文将解释如何实现它。第 1 部分将关注后端，然后在第 2 部分回顾前端。

# Django 认证库

Django 有大量的[认证库](https://djangopackages.org/grids/g/authentication/)。许多人做相同或相似的事情。有些维护得不好，有些维护得非常好并有记录。如果你想做些功课，以下是我的建议供你参考:

*   [姜戈所有授权](https://django-allauth.readthedocs.io/en/latest/)
*   [Dj Rest Auth](https://dj-rest-auth.readthedocs.io/en/latest/introduction.html) (不要和它的前身 Django Rest Auth 混淆)
*   Django Rest 框架简单 JWT (不要与 Django Rest 框架 JWT 混淆)
*   [Django Outh 工具包](https://django-oauth-toolkit.readthedocs.io/en/latest/)
*   Django Rest 框架文档还列出了更多

在某些时候，您可能还会遇到 OAuth、OAuth2 和 OpenID Connect。这些是更深层次的话题。请记住

> **OAuth** 是一种[开放标准](https://en.wikipedia.org/wiki/Open_standard)用于访问[委托](https://en.wikipedia.org/wiki/Delegation_(computer_security))，通常用于互联网用户授权网站或应用程序访问他们在其他网站上的信息，但不需要给他们密码。

所以 OAuth 是一个标准，你可以在这个标准中使用 JWT。还有一个叫做 Auth0 的第三方服务，我经常把它和 OAuth 搞混…所以要小心那个。当然，如果你想了解第三方工具，可以随意查看 [Auth0](https://auth0.com/) 和 [Okta](https://www.okta.com/) 。

综合考虑，我选择了 [Dj Rest Auth](https://dj-rest-auth.readthedocs.io/en/latest/introduction.html) 。这是我发现的最符合我对库“正常工作”的偏好的一个。实际上，你可以配置它来使用简单 JWT 和全授权库，所以它们可以很好地结合在一起。此外，当您准备好使用社交认证(例如脸书或 Github 登录)时，文档可以带您进入这一阶段。

所以事不宜迟，让我们做一些编码。

# 设置 Django 后端

我不会深入介绍如何设置 Django 应用程序的基础知识，这是假设的。我也不会进入非常简单的`dj-rest-auth`安装；我相信你会遵循优秀的[文档](https://dj-rest-auth.readthedocs.io/en/latest/installation.html)。

安装完`dj-rest-auth`(不要忘记应用迁移)之后，您将可以立即访问各种剩余的[端点](https://dj-rest-auth.readthedocs.io/en/latest/api_endpoints.html)，例如`/login`和`/logout`。在 Django Rest 框架可浏览 API(通常位于`localhost:8000`)中测试这些是个好主意。

如果您在这一阶段遇到错误，请确保您已经在`settings.py`中添加了所有必要的库，并将所有东西安装到您的虚拟环境中。

```
INSTALLED_APPS = [
    'django.contrib.auth',
    'django.contrib.admin',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'django_extensions',
    'corsheaders',
    'dj_rest_auth',
    'rest_framework',
    'rest_framework.authtoken',
    'django.contrib.sites',
    'allauth',
    'allauth.account',
    'allauth.socialaccount',
    'dj_rest_auth.registration',
    'sslserver', # we'll discuss this one later
    'users' # This is just the name of my django app
]
```

请注意，该列表包括`allauth`个社交账户；这实际上是为了让`dj-rest-auth`的注册端点能够工作。此外，请确保在您的虚拟环境中安装了`djangorestframework-simplejwt`。

好，现在我们将明确地开始使用 JWT。在`settings.py`中应用的设置有:

```
 ...REST_FRAMEWORK = {
    'DEFAULT_AUTHENTICATION_CLASSES': (
        'rest_framework.authentication.TokenAuthentication',
        'dj_rest_auth.jwt_auth.JWTCookieAuthentication'
    ),
    'DEFAULT_SCHEMA_CLASS': \
        rest_framework.schemas.coreapi.AutoSchema', 'DEFAULT_PERMISSION_CLASSES': [
        'rest_framework.permissions.IsAuthenticated'
    ]
}REST_SESSION_LOGIN = False
REST_USE_JWT = True
JWT_AUTH_COOKIE = 'jwt-access-token'           # you can set these
JWT_AUTH_REFRESH_COOKIE = 'jwt-refresh-token'  # to anything 
JWT_AUTH_SECURE = True
CORS_ALLOW_CREDENTIALS = True
CORS_ALLOWED_ORIGINS = ['https://example.com']...
```

这样，我们关闭了会话认证，告诉 Django 使用 JWT，设置我们将发送到浏览器的 cookies 的名称，设置 CORS 接受嵌入凭证的请求，最后设置 https。

最后一点产生了一个问题:我们应该如何在开发环境中使用 SSL/TLS？当然，我们可以不设置`JWT_AUTH_SECURE`，但结果是我们无论如何都需要 https。这使我们……

# 饼干(nom nom)

好吧，支持争议。我认为大多数人似乎讨厌 JWT，因为开发人员一直将访问令牌和刷新令牌放在 LocalStorage 中。不要这样。也不要把它们放在普通饼干里。

需要做的是将令牌放在一种称为 httpOnly cookie 的特殊类型的 cookie 中。Javascript 不能读取这种类型的 cookie，这提供了一些针对 XSS 的保护。

幸运的是，`dj-rest-auth`抽象了几乎所有关于 httpOnly cookies 的东西(嗯，可能没那么幸运，我选择这个库很大程度上是因为这个)。然而，没有涵盖的是如何使用这些 cookies 使解耦的前端应用程序与后端交互。现在出现了一些重要的差异，在使用可浏览 API 时，您不必担心这些差异:

*   `dj-rest-auth`的 httpOnly cookies 喜欢 https
*   这些域必须是“相同站点”

这些要点归结起来就是我们需要 https，甚至在我们的开发环境中，我们必须使用相关的正确域名，即子域。为了使用我们的开发环境，我们需要欺骗这些。在生产中，这不会是一个问题，因为我们将有 SSL 和真正的 DNS。

*注意:我想我不需要告诉你在生产中使用 https。如果你觉得 Nginx 有点混乱，查一下*[*caddy server*](https://caddyserver.com/)*，默认是 https。*

## 如何创建带有自定义域的 https 开发环境

在后端，有一个工具可以做到这一点:`[django-sslserver](https://github.com/teddziuba/django-sslserver)`。将其安装到您的环境中，添加到`INSTALLED_APPS`，并用`runsslserver`替换`runserver`。现在只需告诉您的浏览器可以接受自签名证书，您就可以开始了。魔法。

我们还需要我们的认证后端将 API 请求视为来自同一个根/apex 域，以便它正常工作。这可以通过在 hosts 文件中应用本地域来实现。在 Linux 中，它位于`/etc/hosts`。

将以下内容添加到`/etc/hosts`，或您的操作系统的任何主机文件:

```
127.0.0.1     api.example.com
127.0.0.1     example.com
```

我们仍然需要包括端口，但是你现在应该能够在`https://api.example.com:8000`访问可浏览的 API，在`https://example.com:3000`访问前端(或者你的前端正在监听的任何端口)。

# 最后的后端任务:定制中间件的飞溅

好了，我们马上就要完成后端了。然而，正如这个 [Github 问题跟踪](https://github.com/iMerica/dj-rest-auth/issues/97)条目所讨论的，关于`dj-rest-auth`有一个症结。希望在以后的版本中能解决这个问题，但是现在，问题是我们的浏览器将在请求头中发送我们的访问令牌。

这对于普通的请求来说没问题，但是如果我们希望刷新我们的访问令牌，`dj-rest-auth`要求刷新令牌在主体中发送，而不是在头中发送。在 Github 发行版中，建议使用以下中间件将标记从头部移动到主体。

在您的 Django 应用程序中，创建一个`middleware.py`文件。添加此代码:

然后，您必须将这个中间件添加到`INSTALLED_APPS`:

```
MIDDLEWARE = [        ... 'yourappname.middleware.MoveJWTRefreshCookieIntoTheBody',
]
```

完成所有这些设置后，Django 现在应该可以在您的开发环境中运行一个功能正常的 JWT 服务器了。

*在本文的* [*第二部分*](https://johnckealy.medium.com/jwt-authentication-in-django-part-2-implementing-the-frontend-7ea3d6e16bf4) *中，我们将探讨一些前端设置，并完成完整的堆栈实现。*