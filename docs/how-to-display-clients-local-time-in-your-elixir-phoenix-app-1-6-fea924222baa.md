# 如何:在您的 Elixir/Phoenix 应用程序(1.6 以上)中显示客户端的当地时间

> 原文：<https://medium.com/geekculture/how-to-display-clients-local-time-in-your-elixir-phoenix-app-1-6-fea924222baa?source=collection_archive---------9----------------------->

![](img/f50be4e83d4963770eb64973f80fb7a9.png)

Undraw’s time management

# 背景

随着我们走向 2021 年底，我一直在推动将首批几个早期访问邀请代码发送给那些注册了变形早期访问发布的人——这样做，我想找到一种快速而简单的方法来显示某人的当前时间，无论他们在世界的哪个地方。

> 对于那些刚刚收听的人来说，[变形](https://metamorphic.app)改变了我们在线联系和分享的方式。我设计它的时候，考虑到了我的家庭，提供了一个安全、有趣、简单的联系和分享的方式；同时与监控资本主义的破坏性力量绝缘，监控资本主义是主要社交网络、视频聊天和搜索公司的动力和驱动力。

通常，在开发中显示本地时间非常容易，因为您的“服务器”和“客户机”(您和您的计算机)在同一个位置。然而，在生产中，由于客户机不再位于服务器所在的位置，这往往会变得更加复杂。

任何使用*时间*的人，或者作为一名开发者，都知道*时间*是一个极其复杂的编程概念(问题、哲学等)。).我们不会进入那些本质的细节，谢天谢地，我们可以依靠精彩的库来为我们做这些工作。

但是，即使有了这些出色的库，用客户的当地时区显示他们的时间在生产中仍然很复杂。

所以，我将向你展示我用变形做这件事的快速简单的方法。

# 先决条件

在这篇文章中，我们将做如下假设:

*   你有一个 [Phoenix](https://phoenixframework.org) 的应用程序启动并运行(1.6 以上)
*   你有 17.5 以上的凤凰城直播
*   你有仙丹 1.12+和 OTP 24+
*   您已经安装了 [Timex](https://hex.pm/packages/timex) 库(或者了解如何在 Elixir 或另一个库中使用等效的时间相关函数)

好吧，我们开始吧。

# 方法

假设我们有一个登录用户的仪表板或主页，在这个页面上，我们向他们显示当地时区的实时时钟。

有很多不同的方法可以做到这一点，但是假设我们不想用任何细节来打扰他们，我们也不想在数据库中保存人们的时区(为了隐私或简单起见)。

为了实现这一点，我们将利用 Phoenix LiveView 令人难以置信的 [on_mount/1](https://hexdocs.pm/phoenix_live_view/Phoenix.LiveView.html#on_mount/1) 和 [attach_hook/4](https://hexdocs.pm/phoenix_live_view/Phoenix.LiveView.html#attach_hook/4) 函数，通过一个简单的 JavaScript 钩子获取客户端的本地时区，并在挂载 LiveView 页面时将其分配给我们的 socket。

我们走吧。

# 第一步

首先，让我们创建我们的`on_mount/4`函数，我们将用我们的`on_mount/1`回调函数调用它(我们正在定义我们的`on_mount/1`回调钩子将如何工作):

```
# your_app_web/hooks/local_time_zone_hook.ex
# You can organize your code however you like.defmodule YourAppWeb.Hooks.LocalTimeZoneHook do
  @moduledoc false
  import Phoenix.LiveView

  def on_mount(:default, _params, _session, socket) do
    {:cont,
      socket
      |> attach_hook(:local_tz, :handle_event, fn
        "local-timezone", %{"local-timezone" => local_timezone},
          socket ->
            # Handle our "local-timezone" event and detach hook
            socket =
              socket
              |> assign(:local_timezone, local_timezone)
            {:halt, detach_hook(socket, :local_tz, :handle_event)} _event, _params, socket ->
          {:cont, socket}
      end)
    }
  end
end
```

好的，本质上我们所做的是在我们的`on_mount/4`回调中调用`attach_hook/4`，我们稍后将调用我们的`on_mount/1`来挂钩到我们的 LiveView 的挂载过程。

我们附加的钩子叫做`local_tz`(或者在我们的 JavaScript 文件中叫做`LocalTz`)。

在不太了解我们的钩子的情况下，我们已经知道它将发送一个名为“local-timezone”的事件，我们将对该事件进行模式匹配，并将其作为`local_timezone`分配给我们的套接字。

现在让我们编写 JavaScript 钩子。

# 第二步

为了简单起见，我们将在我们的`app.js`文件中编写我们的钩子，但是您可以组织您的代码来满足您的应用程序的需要。

在您的`app.js`中，您应该已经有了这样的内容:

```
# your_app_web/assets/js/app.js...
let liveSocket = new LiveSocket('/live', Socket, {
  hooks: Hooks,
  ...
```

这一行连接了我们编写的任何 JavaScript 挂钩，并将它们添加到我们的套接字中。

因此，在此之上，让我们添加我们的`LocalTz`钩子:

```
# your_app_web/assets/js/app.js...
Hooks.LocalTz = {
  mounted() {
    let localTz =
      Intl.DateTimeFormat().resolvedOptions().timeZone
    this.pushEvent("local-timezone", {local_timezone: localTz})
  }
}let liveSocket = ...
```

这就是我们的钩子！

如您所见，我们使用了一些很棒的 JavaScript 来(1)获取客户端的本地时区，(2)将其分配给`localTz`，以及(3)向服务器发送一个名为“local-timezone”的事件，其中包含我们想要的相关数据。

完成后，我们可以进入最后一步。

# 第三步

在最后一步中，我们将处理事件并启用我们的实时时钟。根据您的应用程序的需求，有很大的定制和扩展空间(这里我们不会深入讨论)。

我们需要做的是用`on_mount/1`函数调用我们的钩子，连接我们的时钟，并显示我们的时钟(我们假设您有身份验证或您的应用程序需要的任何东西)。

让我们来看看:

your_app_web/dashboard_live/index.ex

这是怎么回事？

在第 5 行，我们使用`on_mount/1`回调来调用我们的钩子，并将客户端的本地时区弹出到我们的 socket 上的 assign 中。现在，这并不一定会超过我们其余的 mount 函数，所以我们开始我们的`@date`赋值，而不参考客户机的本地时区。

然后，如果套接字连接，我们开始每秒向自己发送一条消息，并更新我们的时钟:`Process.send_after(self(), :tick, 1000)`。我们在第 41 行的`handle_info/2`函数中处理它(在该函数中，我们再次向自己发送消息，以创建时钟的循环“循环”或“滴答”)。最后，在我们的`handle_info/2`结束之前，我们调用我们的`put_date/1`函数，它只是更新我们的`@date`赋值。

需要特别注意的是，现在，在我们的`put_date/1`函数中，我们可以调用`Timex.now(socket.assigns.local_timezone)`，我们将从客户端插入本地时区，我们的钩子在`local_timezone` assign 下为我们将该时区放入套接字。

最后，由于我们使用的是`phx-hook`，我们需要确保在 HTML、`id="local-timezone"`和`phx-hook="LocalTimeZone"`中分配一个惟一的 DOM id。

# 奖金(第 6 步)

现在，在初始安装和我们时钟的第一次“滴答”之间很可能有一点延迟(所以在时钟切换到客户机的本地时间之前，您会看到一会儿 UTC 时间)。

因此，让我们稍微修改一下，为改进 UX 打下基础:

不错！让我们看看我们改变了什么:

*   `assign(:timezone_loaded, false)` —在第 17 行，我们用一个标志替换了我们的`@date`赋值，以表明我们是否已经加载了客户端的本地时区信息。
*   `lines 28 — 32` —这里，我们在 HTML 中添加了一个简单的`if else`块，用于显示加载客户端本地时区前后的信息。
*   `maybe_update_assigns_for_clock(socket)` —在第 47 行，我们在`handle_info/2`回调中调用了一个新的`maybe_update_assigns_for_clock/1`函数，以便在本地时区信息从我们的钩子中加载后更新我们的套接字。
*   `maybe_update_assigns_for_clock/1` —在第 50–61 行，我们实现了一个简单的`if else do end`块，在这里我们检查`:date`键的`socket.assigns`图并相应地继续。这个函数的两个分支都通过调用我们现有的`put_date/1`函数来确保我们的时钟继续运行。

这样，在加载本地时区信息之前，我们已经用加载消息代替了查看 UTC 时间。您可以在此基础上进行扩展，根据应用程序的需求改进和进一步定制 UX。

# 结论

我们完事了。只需几行简单的 JavaScript 代码和 Phoenix LiveView 的新`on_mount/1`功能，我们就可以轻松显示每个客户所在时区的时间。

感谢您的关注，如果您正在为如何处理客户的当地时间而绞尽脑汁，我希望这能帮助您，或者激励您创建更棒的东西。

> *如果你有兴趣了解更多关于*变形*的信息，那么就跟随我们的公司* [*播客*](https://metamorphic.transistor.fm) *和* [*报名参加邀请码*](https://metamorphic.app) *来我们的早期接入发布吧——我们的网络生活即将迎来一场变革！*

永远欢迎改进和想法，谢谢！

💚标记