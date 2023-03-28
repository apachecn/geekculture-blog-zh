# 如何:在尊重隐私的同时限制登录尝试次数

> 原文：<https://medium.com/geekculture/how-to-rate-limit-log-in-attempts-while-respecting-privacy-a1ae220640e8?source=collection_archive---------35----------------------->

![](img/b14b8abf076001d19f9014d5ee5e1c60.png)

Approaching log in limit message on Metamorphic. © Moss Piglet

# 背景

如果你一直在关注我最近的几篇帖子，那么你会意识到我目前正在开发一种专注于隐私的替代大型科技社交网络(或社交媒体)的产品，名为*变形*。

在我进行的过程中，我分享了我学到的一些我认为很酷或者可能对其他人有用的东西。

在这种情况下，我决定对某个人可以尝试登录他们的帐户的次数进行限制，以此来给潜在的恶意行为者增加一些摩擦。

与此同时，我也在决定如何处理人们以短暂而持久的方式上传的记忆(图像)。这意味着将加密的图像上传到云提供商，用于存储负载；然后在一个人的会话期间实时处理我们服务器上的内存的解密、临时存储和应用程序内使用(人们的内存是不对称加密的，除了内存的所有者和他们选择与之共享的人之外，任何人都不能解密)。

当我思考这两个场景时，我不断地回到一件事情上， [Erlang Term Storage](https://erlang.org/doc/man/ets.html) (ETS)。

所以我开始研究 ETS，发现了 Chris McCord 在[造船厂](https://dockyard.com/blog/2017/05/19/optimizing-elixir-and-phoenix-with-ets)写的这篇精彩的博客，并决定尝试一下登录速率限制器(以及后来一个更复杂的短暂应用内内存解密实现)。

# 先决条件

在这篇文章中，我们将做如下假设:

*   你已经有了一个 [Phoenix](https://phoenixframework.org) 应用程序，并且正在运行(~ 1.5.9)
*   您已经安装了 phx_gen_auth(除非您使用的是 Phoenix 1.6 以上版本)
*   您已经启动并运行了一个身份验证系统(或者可以理解您自己场景中的等效情况)
*   可选:登录或登录命名(当命名文件或在面向人的场景中讲话时，我在两者之间切换)
*   可选:我用一个人/几个人来代表一个/几个用户

# 信用

特别感谢[船坞](https://dockyard.com/)和 Chris McCord 的[博客文章](https://dockyard.com/blog/2017/05/19/optimizing-elixir-and-phoenix-with-ets)，这是一个巨人，我们特别站在他的肩膀上看这个简短的指南(还有所有其他的巨人… Erlang/Elixir/Phoenix 等等)。).如果你需要技术帮助，我强烈推荐你去看看。

事不宜迟，让我们开始吧。

# 第一步

第一步，让我们创建一个 [GenServer](https://elixir-lang.org/getting-started/mix-otp/genserver.html) 处理器来处理我们的登录限制器的编排。

在应用程序代码的 web 部分，让我们创建一个名为“extensions”的文件夹，并将我们的`max_login_processor.ex`文件放入其中。因此，您的文件结构可能如下所示:

```
your_app_web
│   ...
└───extensions
│   │   max_login_processor.ex
│   ...# your_app_webb/extensions/max_login_processor.ex
```

您可以随意命名您的文件夹，例如，如果将它与您的身份验证控制器放在一起，您可能会感觉更好。使用 extensions 文件夹只是为了帮助组织我们的代码。

那么，代码呢？

对于您的`max_login_processor.ex`文件，您可以复制 [Chris McCord 的示例](https://dockyard.com/blog/2017/05/19/optimizing-elixir-and-phoenix-with-ets)并进行以下更改:

```
defmodule YourAppWeb.Extensions.MaxLoginProcessor do
  @moduledoc """
  A GenServer to limit log in attempts
  to 5 every hour.
  """
  use GenServer
  # remove the require Logger or not @max_per_hour 5
  @sweep_after :timer.hours(1)
  @tab :rate_limiter_requests ... def log(sid) do
    case :ets.update_counter(@tab, sid, {2, 1}, {sid, 0}) do
      count when count > @max_per_hour -> {:error, :rate_limited}
      count -> {:ok, count}
    end
  end def sweep_on_login(sid) do
    case :ets.lookup(@tab, sid) do
      [{session_id, _}] ->
        :ets.delete(@tab, session_id)
      [_] ->
        :ok
    end
  end ## Server def init(_) do
    # Remove debug logging if removing Logger from above
    ...
  end ...
end
```

好吧，这里有一个很小的改动:

*   改变了 uid -> sid 来表示我们正在使用一个人的`session_id`
*   将预定扫描更新为 1 小时(`@sweep_after :timer.hours(1)`)
*   添加一个`sweep_on_login/1`功能，当一个人成功登录时清除会话的登录计数(我们不希望人们把自己锁在外面)

完成后，进入第二步，也是最后一步。

# 第二步

打开您的`person_session_controller.ex`文件(或等效文件),我们将使用`case`语句包装我们的日志逻辑:

```
#your_app_web/controllers/person_session_controller.exdefmodule YourAppWeb.PersonSessionController do
  ...  
  alias YourAppWeb.Accounts
  alias YourAppWeb.Extensions.MaxLoginProcessor 
  ... def create(conn, %{"person" => person_params}) do
    %{"email" => email, "password" => password = person_params
    %{"_csrf_token" => session_token} = conn |> get_session() case MaxLoginProcessor.log(session_token) do
      {:ok, count} ->
        case Accounts.get_person_by_email_and_password(email,
                      password) do
          {:ok, person} ->
            MaxLoginProcessor.sweep_on_login(session_token)
            ...
          # Your errors here
          # You can add logic to display remaining log in attempts
          # alongside your existing auth error logic.
          # Your auth error logic should be redirecting back
          # to log in.
          ...
        end  
      {:error, :rate_limited} ->
        conn
        |> put_flash(:error, "Your message here.")
        |> redirect(to: Routes.person_session_path(conn, :new)
    end
  end ...
end
```

就是这样！

现在，正如我在评论中提到的，你可以微调你向人们显示的信息，但这是它如何工作的总体结构:

1.  如果一个人成功登录，那么我们调用我们的`sweep_on_login/1`函数，传递这个人的`session_token`(我们从`conn`模式匹配而来)，清除任何现有的登录尝试。这确保了一个人可以随心所欲地来去，而不会将自己锁定在帐户之外。
2.  如果一个人登录失败的次数太多，那么他的帐户将被锁定 1 小时(我们在`max_login_processor`中安排的扫描时间)。

# 说明

我们选择这种方法有几个原因:

*   增加一个人的暴力尝试的摩擦
*   给意外将自己锁在外面的人增加最小的摩擦
*   通过使用`session_id`而不是 IP 地址来尊重人们的隐私
*   是“盲的”,因为它适用于所有会话，而不管帐户是否存在

稍微详细说明一下，如果有人试图暴力破解一个人的帐户(只有他们可以通过他们的密码访问)，那么暴力攻击者将不得不不断地开始新的会话和/或等待一个小时。这不是不可能的，但它使攻击需要更多的工作(这是游戏的名称)。

另一方面，如果一个人不小心把自己锁在外面，那么他们可以开始一个新的会话(比如一个匿名窗口)或者等待 1 个小时。再说一次，我们不想让人们的经历更加痛苦，以防有人试图闯入他们的帐户(已经有其他防御措施，如强密码、可选的 2FA 和“盲目”错误消息)。

最后，通过使用`session_id`,我们尊重人们的隐私，不提取和记录(即使是暂时的)他们的 IP 地址，这可能与他们的位置直接相关(除非在一个良好的 VPN 后面)。此外，当我们将`session_id`方法与“盲”错误消息(不显示某人是否拥有我们的帐户的消息)结合使用时，它可以进一步保护人们的隐私，因为如果攻击者试图闯入现有或不存在的帐户，他们也会得到相同的错误。

# 结论

原来如此！

如果您正在考虑在您的 Elixir/Phoenix 应用程序中限制登录尝试，并且希望尊重人们的隐私，希望它能帮助您。

你可以很容易地定制`max_login_processor`来进一步满足你的特定需求。

> 如果你有兴趣了解更多关于*变形*的信息，那么跟随我们的公司[播客](https://coretheory.transistor.fm/)或者在 [Patreon](https://www.patreon.com/coretheory) 上支持我们，并保持关注——我们的网络生活即将发生变革！

永远欢迎改进和想法，谢谢！

💚标记