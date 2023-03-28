# 使用 Facebook Messenger 聊天机器人和 NodeJS 安排约会

> 原文：<https://medium.com/geekculture/appointment-scheduling-with-facebook-messenger-chat-bots-and-nodejs-53f1a539b283?source=collection_archive---------21----------------------->

![](img/91bf794a5bcbaee1cf3b291b993a01d9.png)

在这篇文章中，我解释了我们如何使用 Facebook Messenger API 创建一个集成了**预约**的聊天机器人。这是一个 NodeJS 实现，但可以很容易地用任何其他现代语言复制。

脸书非常灵活，提供了许多很酷的功能。messenger 可以添加到您自己的脸书页面，也可以直接嵌入到您自己的网站或应用程序中。出于教育目的，我们将在脸书页面上使用它。完整的代码位于本文底部的 GitHub 库。

这是它如何工作的基本演示:

![](img/45bf4c118554e287f3cb856ea651a439.png)

# 获取 API 凭据

要开始使用 Facebook Messenger API，你可以跟随官方指南。你需要获得三个重要的凭证:`App Secret Key`、`Page Access Token`和`Callback user token`。最后一个令牌是用户定义的值。

# 履行

在其中一个步骤中，您必须提供一个 webhook URL 到您的服务器，这允许脸书验证连接，但也验证您的用户定义的令牌:

```
// GET request
router.get('/spurwing-fbbot/', (req, res) => {
  // verify token and send back the challenge
});
```

一旦你的脸书应用程序被创建并且网页挂钩被脸书成功验证，我们就可以开始实现和测试我们页面的信使和聊天机器人:

```
// POST request
router.post('/spurwing-fbbot/', async (req, res) => { verifyRequestSignature(req, res) // make sure it really is Facebook's message for (const e of req.body.entry) {
    if (e.messaging)
      for (const m of e.messaging) {
        await fb_msg_process(m.sender.id, m.message)
      }
  } res.send({success:1})
});
```

上面的代码是一个非常简单的路由器实现，用于通过 messenger 接收用户消息。接下来，我们需要处理用户文本并正确回复:

```
async function fb_msg_process(senderId, msg) { // default fall-back message
  let resp = {text: "I don't know what you mean :("} if (msg && msg.text) {
    let text = msg.text.toLowerCase();
    if (msg.quick_reply && msg.quick_reply.payload)
      text = msg.quick_reply.payload; switch(true) {
      case /^book$/.test(text):
        resp = await fb_msg_process_funcs.book(text);
        break;
      case /^book_day:.+$/.test(text):
        resp = await fb_msg_process_funcs.book_day(text);
        break;
      case /^book_slot:.+$/.test(text):
        resp = await fb_msg_process_funcs.book_slot(text);
        break;
    }  
  } fb_msg_reply(senderId, resp) // send a reply back}
```

上面的代码根据上下文解析和处理接收到的消息。顶部的动画 gif 显示了这种逻辑的实际应用。

**预约和调度逻辑**由我们的 Spurwing API ( [NodeJS 库](https://github.com/spurwingio/Spurwing-API-NodeJS-Library))提供。它允许我们列出所有可用的日期，然后列出某一天所有可用的时间段，最后在选定的时间段预约。该实现的完整代码位于我们的 [GitHub 库](https://github.com/Spurwingio/Chat-Bots/tree/main/Facebook/NodeJS)的`index.js`中。

# 结论

这是一个使用 Facebook Messenger API 的非常简单而有效的聊天机器人实现。但它确实遗漏了一些关键细节:

*   所有日期和时间都相对于您的服务器，而不是相对于用户的时区。脸书有高级消息传递功能，您可以启用它来接收用户的实际时区。
*   或者，你可以在聊天中询问用户的时区。
*   快速回复按钮数量有限。但是可用天数和/或时隙数可能会超过这个限制。应该实现自定义逻辑来提供更灵活的调度选项。

由开发人员决定如何处理用户的时区和快速回复输入。后者可以通过手动输入一个时间段并给出关于其可用性的反馈来实现，甚至可以使用 NLP 策略来进行更复杂的语言解析。但是如果你是一个编程新手，保持简单和容易。

欲了解更多预订和日历解决方案，请访问我们的 [Github 账户](https://github.com/Spurwingio/)。