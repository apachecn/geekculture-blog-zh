# 如何:在保护每个人的隐私的同时添加针对不良行为者的保护

> 原文：<https://medium.com/geekculture/how-to-add-protection-against-bad-actors-while-protecting-everyones-privacy-4317130ea2b3?source=collection_archive---------20----------------------->

![](img/459b3a56d84b152b1fc1b4db548b4e97.png)

Undraw hacker mind

# 背景

如果你一直在关注，那么你会知道我一直在构建一个以隐私为中心的脸书替代方案(呃… Meta？).这不仅仅是一个简单的克隆，事实上它完全背离了公司的一切。

它被称为[变形](https://metamorphic.app)，我目前正准备发出最初的几个早期访问邀请代码(如此接近！).这是一种自举式的努力，一种真正的爱的劳动——我建立它是为了让我的家人和所爱的人能够联系和分享，而不会削弱我们的生活(大致总结一下)——并且由于服务的自举性质，我必须随着收入的增加来扩大硬件规模(我的跑道比我打字的桌子还短)，这意味着我目前在*非常低效的*硬件上运行生产服务器。

它能工作的事实让我总是想赞美 Elixir、Phoenix 和 OTP，但它让我在服务开始之前就开始思考它有多脆弱。

我一直试图把它赶出我的脑海，因为我已经走出了我的方式，不让服务器拉人们的 IP 地址(即使是暂时的)，见[这篇文章](/geekculture/how-to-rate-limit-log-in-attempts-while-respecting-privacy-a1ae220640e8)关于使用会话来限制登录尝试，但它不断回来。我需要做点什么。

多亏了一篇 [Elixir 论坛帖子](https://elixirforum.com/t/dealing-with-bots/43736/2?u=f0rest8)，我了解了一个名为 [plug_attack](https://github.com/michalmuskala/plug_attack) 的库，并意识到我可以使用这个库和一些简单但强大的散列来增加对“不良行为者 IP 地址”的保护，而无需存储 IP 地址(即使是临时的)。

注意:你很可能也想使用 [remote_ip](https://github.com/ajvondrak/remote_ip) ，在奖励步骤 6 中提到过。

所以，废话不多说，让我和你们分享一下我是如何做到这一点的。

# 先决条件

在这篇文章中，我们将做如下假设:

*   你有一个 [Phoenix](https://phoenixframework.org) 的应用程序启动并运行(1.6 以上)
*   你有仙丹 1.12+和 OTP 24+

对于这个简短的教程来说，这就是你所需要的全部，但我假设你有一个更成熟的应用程序，它具有身份验证和其他业务逻辑，尽管它不会在这里出现。

我认为您也可以使用低至 Elixir 1.4 的版本(尽管您也很可能使用较低的 OTP 版本)，但是如果您使用更低的版本，那么您需要遵循一个额外的步骤来确保`plug_attack`在您的应用程序之前启动(并且很可能更改您的散列函数)。

```
# Only necessary if using Elixir 1.3
def application do
  [applications: [:plug_attack]]
end
```

让我们开始吧。

# 第一步

首先，在`mix.exs`中将`plug_attack`添加到您的依赖项中，如果使用的是伞式应用程序，那么相应地添加(在我的例子中，我在我的 web 部分中添加了依赖项列表，因为它直接处理 web 请求)。

```
# mix.exs
...
defp deps do
  [
    ...
    {:plug_attack, "~> 0.4.3"},
    ...
  ]
end
```

然后，从您的终端运行`mix deps.get`(您可能想要检查[十六进制包](https://hex.pm/packages/plug_attack)以查看最新版本)。

下一个。

# 第二步

接下来，我们将在应用程序中添加一个新的环境变量。如果你还没有一个`.env`文件，你需要在你的应用程序的根目录下创建一个。然后，确保你也将`*.env`添加到你的`.gitignore`文件或者类似的文件中(**要格外小心，不要**将你的环境变量推到你的源代码控制中)。

在你的`.env`文件中添加`export PLUG_ATTACK_IP_SECRET=randombytes`。你需要为这个秘密生成一些随机字节。可以这样做:`:crypto.strong_rand_bytes(12) |> Base.encode64()`。

有了这样的安排，我们就可以继续了。

# 第三步

接下来，让我们创建我们的`plug_attack.ex`模块。这将是一个简单的例子，您可以在您的开发环境中立即测试，您可以根据您的应用程序来组织它。对我来说，我在我的 web 文件夹中创建了一个`plugs`文件夹，但这取决于你！

解决了代码组织后，您可以通过简单地复制/粘贴“速率限制头”部分的演示开始，并做一些小的调整:

Protect against “bad actor IP’s” while protecting everyone’s privacy

好吧，这是怎么回事？实际上相当多，但是为了我们的缘故，重要的部分如下:

*   `@alg :sha512` —这将在我们的散列函数中使用。
*   `@ip_secret System.fetch_env!("PLUG_ATTACK_IP_SECRET")` —这将在我们的哈希函数中使用。
*   `hash_ip/2` —这是我们的散列函数，它采用一种算法`alg`和转换后的 IP 地址`ip`(您可以用不同的名称来表示，以便更加明确)。然后，我们使用 Erlang 的`:crypto.mac/4`函数，使用我们提供的算法`:sha512`和`@ip_secret`对转换后的 IP 地址进行散列。
*   `convert_ip/1` —该函数从`conn.remote_ip`中获取 IP 地址，并将其从 IO 数据转换成可以在我们的哈希函数中正确使用的字符串。
*   `rule "throttle by ip"`——这个“规则”使我们能够通过 IP 地址限制所有请求，以便我们能够快速测试它在我们的开发环境中是否工作。您可以稍后添加一个类似于`rule "allow local", conn do`的规则来总是允许来自`localhost`的请求。

因此，我们使用`:sha512`算法和来自`@ip_secret`的 salt 来确定性地散列传入的 IP 地址，然后将该散列临时存储在我们的 Erlang Term Storage (ETS)表中。

然后，我们将散列存储`period: 60_000`毫秒(1 分钟)，并在同一分钟内阻止来自该散列 IP 地址的任何超过 10 的请求`limit: 10`。

好的，我们就快到了。在我们的`application.ex`文件中，我们需要将我们的 ETS 存储添加到我们在上面的 PlugAttack 规则中列出的监管树中，`storage: {PlugAttack.Storage.Ets, YourAppWeb.PlugAttack.Storage}`:

```
# your_app_web/application.exdefmodule YourAppWeb.Application do
  @moduledoc false
  use Application

  def start(_type, _args) do
    ...
    children = [
      ...
      # Start PlugAttack storage
      {PlugAttack.Storage.Ets, name: YourAppWeb.PlugAttack.Storage, clean_period: 60_000},
      ...
    ]
    ...
  end
  ...
end
```

仅此而已。同样，你可以在 PlugAttack [文档](https://github.com/michalmuskala/plug_attack)中了解更多关于你的储物选择和启动策略。

# 第四步

转到应用程序 web 部分的`router.ex`文件，将您的插件添加到适当的管道中:

```
# router.ex
defmodule YourAppWeb.Router do
  use YourAppWeb, :router ...
  alias YourAppWeb.Plugs.PlugAttack
  ...
  pipeline :browser do
    plug PlugAttack
    ...
  end
  ...
end
```

就是这样！我们给我们的`PlugAttack`模块起别名，然后把塞子放进我们的`:browser`管道，在我们做任何其他事情来确保请求被允许通过之前，管道通过它。

你可以通过点击刷新按钮进行测试，直到你点击`Forbidden`屏幕。

现在，正如您可能已经猜到的那样，可以对它进行很大的修改和扩展，以满足您的需要。重要的是，我们现在知道如何获取一个传入的 IP 地址，散列它以保护个人隐私，临时存储它(这都是短暂的！)，然后相互比较散列来决定是否允许请求。

很酷，但是我们实际上还没有完成。让我们看看我们可以采取的另一个步骤，即利用由类似命名的算法启发的`fail2ban/2`函数。

# 奖励步骤 5

对于这最后一步，我们将用实现`PlugAttack.Rule.fail2ban/2`算法的规则来替换我们的`rule "throttle by ip"`。

来自[文档](https://github.com/michalmuskala/plug_attack):

> 这是为了尽早发现行为不端的客户，并持续更长时间。“密钥”用于区分不同的客户端，例如，您可以使用“conn.remote_ip”进行每个 ip 的跟踪。如果“键”为假，则跳过该动作，并评估下一个规则。

Protect against “bad actor IP’s” while preserving everyone’s privacy with fail2ban

因此，我们交换了我们的规则，将禁令设置为 90 秒，`ban_for: 90_000`。这是为了让我们可以在开发中轻松地测试它。对于生产，您可能想要增加`:ban_for`周期。

说到生产…

# 奖金第 6 步

如果您的生产服务器位于 Heroku 的路由器或其他转发请求的主机之后，那么您可能还需要将 [remote_ip](https://github.com/ajvondrak/remote_ip) 添加到您的依赖列表中，并在您调用`plug YourAppWeb.Router`之前将其放入您的`endpoint.ex`文件中。

```
# mix.exs
...
  defp deps do
    [
      ...
      {:remote_ip, "~> 1.0"}
    ]
  end
...# your_app_web/endpoint.exdefmodule YourAppWeb.Endpoint.ex do
  ...
  plug RemoteIp
  plug YourAppWeb.Router
end
```

还有其他配置和使用`remote_ip`的方法，所以如果你需要使用它，我鼓励你仔细阅读[文档](https://hexdocs.pm/remote_ip/RemoteIp.html)。

此外，根据 [Kip Cole](https://medium.com/u/b2a1e629dcd8?source=post_page-----4317130ea2b3--------------------------------) 在 [Elixir Forum](https://elixirforum.com/t/phoenix-channel-get-user-ip-address/36601/4?u=f0rest8) 上的帖子，您可能希望使用`remote_ip`来解决有关 ip 欺骗的问题。

# 2022 年 10 月 30 日更新

为了处理无效的 Unicode 码位错误，一种选择是将`convert_ip/1`函数更新为:

```
defp convert_ip(ip) do
  ip
  |> Tuple.to_list()
  |> Enum.map(fn(i) -> Integer.to_string(i) end)
  |> List.to_string()
end
```

# 结论

就是这样！

感谢您的关注，我希望这能让您了解如何在保护每个人隐私的同时保护您的应用程序。

> 如果你有兴趣了解更多关于*变形*的信息，那么跟随我们的公司[播客](https://coretheory.transistor.fm/)和[报名参加我们的早期接入发布会的邀请码](https://metamorphic.app)——我们的网络生活即将发生转变！

永远欢迎改进和想法，谢谢！

💚标记