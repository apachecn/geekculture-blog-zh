# Django 中的 JWT 认证，第 2 部分:实现前端

> 原文：<https://medium.com/geekculture/jwt-authentication-in-django-part-2-implementing-the-frontend-7ea3d6e16bf4?source=collection_archive---------4----------------------->

![](img/e4420433217922de165a7fb91d92587a.png)

*在* [*上一篇文章*](https://johnckealy.medium.com/jwt-authentication-in-django-part-1-implementing-the-backend-b7c58ab9431b) *中，我们使用* `*django-rest-framework*` *和* `*dj-rest-auth*` *库创建了一个 JWT 认证系统。在本文中，我们将探索如何从解耦的前端与后端进行通信。假定一些先验知识:*

*   *类似 Vue 或 React 的前端框架*
*   *使用@vue/cli 或 Nuxt 等客户端服务前端 app*
*   *状态管理工具(如 Vuex 或 Redux)*
*   *Axios http 客户端*

*要看上一篇，Part 1，点击* [*这里*](https://johnckealy.medium.com/jwt-authentication-in-django-part-1-implementing-the-backend-b7c58ab9431b) *。*

上次，我们使用 Django 为 JWT 创建了一个可靠的后端。人们可能会认为实现一个前端来与它通信应该是微不足道的，尤其是在 npm 上有无数的前端认证库的情况下。然而，我在使用这些库方面还没有取得很大的成功，所以我更喜欢创建自己的自定义登录表单和 API 调用。

实际上，在处理前端客户端时，最重要的事情是尽职调查，打击像跨站脚本(XSS)和跨站请求伪造(CSRF)这样的攻击。在与 JWT 打交道时，前者通常比后者更危险。首先要考虑的是饼干。

# Cookies，以及它们存储的位置

如果您遵循了上一篇教程，您应该有望通过`django-sslserver`设置运行 SSL 的 Django 后端，并通过`/etc/hosts`设置本地 DNS 名称，这允许您在如下地址访问后端

```
https://api.example.com:8000
```

对于前端，我一般用[类星体框架](https://quasar.dev/)或者 [Nuxt](https://nuxtjs.org/) 。像许多包含开发服务器的前端客户端一样，Quasar 在幕后使用 Webpack，webpack 开发服务器有一个设置`https`，您可以将其设置为`true`。这应该可以处理前端 SSL。记得给你所有的网址加一个`s`。

现在是时候在你的前端应用中添加一个端点来访问你的后端了。我将把这个交给你——我们的目标是创建一个向`https://api.example.com:8000/login`端点发送的登录表单，以及一个应该只对经过身份验证的用户可用的经过身份验证的路由。

在你的浏览器中(顺便说一句，我个人支持勇敢的浏览器，Chrome 的所有开发工具，没有任何间谍行为)，打开你的前端地址，在登录表单中输入你的登录信息。这将向 Django 发送一个`POST`请求，Django 将返回您的令牌(一个访问令牌和一个刷新令牌)。

> 此时，奇迹发生了。

如果你打开谷歌 Chrome 或 Brave(其他浏览器会稍有不同)的开发工具，并前往`Application => Cookies`，你可能会发现那里什么也没有。这让我困惑了很久。事实证明，https 的使用和我们在第 1 部分中实现的定制 DNS 现在正在开花结果，cookies 确实被保存了——只是它们没有出现。然而，如果你转到`Network`并点击对`/login`的请求，现在在`Headers/Preview/Timing`标签旁边应该有一个`Cookies`标签。如果您随后检查与请求一起发出的访问头，您应该看到包含了访问令牌。

# 使用状态管理控制前端认证

我更喜欢使用状态管理来编写 Javascript 逻辑，但这不是强制性的。从这一点开始，我的代码片段将与 Vuex 有关，但是如果您使用 React/Redux 之类的东西，这些概念应该仍然有用。我也将使用 [Axios](https://github.com/axios/axios) ，但是你也可以用`fetch()`完成同样的事情。

初始化 Axios 时要设置的关键是设置标志`withCredentials: true`。您可以将该参数传递给任何请求，或者将其设置为默认值，如下所示:

```
import Vue from 'vue'
import axios from 'axios'export default (state) => {

  axios.defaults.withCredentials = true
  axios.defaults.baseURL = process.env.API_URL

  Vue.prototype.$axios = axios
  state.$axios = axios}
```

上述代码可以在引导文件、插件或其他地方找到；这取决于你的前端框架。请注意我是如何将 Axios 添加到状态中的，因此我现在可以在任何地方使用它。

每当`withCredentials = true`时，浏览器都会在其标题中包含令牌。下面是一个放在 Vuex 模块中的典型 Axios 请求示例:

# 离别的思绪

无论你是从头开始编写前端逻辑还是使用一个库，希望这有助于填补一些空白，让你的前端与你的 JWT Django 后端玩得更好。

就下一步而言，还有很多事情要做。无论您选择如何实现前端，您都应该考虑以下几点:

*   为您需要的每个后端端点添加前端表单、按钮和 http 请求(如`/login`、`/logout`、`/password-change`等)。
*   添加令牌刷新端点。登录时删除访问令牌(只需删除`settings.py`中的`JWT_AUTH_COOKIE`条目)并只存储刷新令牌，这也是一个好主意。然后，如果状态改变，您可以通过点击`/refresh-token`端点将访问令牌添加到内存中。
*   在路由器逻辑中添加重定向，以便在用户未经身份验证的情况下将他们发送到登录表单。

感谢您阅读这篇文章。如果您有任何问题或意见，请随时给我写信，地址:[*johnckealy*](https://johnkealy.com)*.dev@gmail.com。*