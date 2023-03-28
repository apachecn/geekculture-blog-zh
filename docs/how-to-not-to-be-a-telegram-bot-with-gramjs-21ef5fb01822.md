# 如何不成为格兰姆斯的电报机器人

> 原文：<https://medium.com/geekculture/how-to-not-to-be-a-telegram-bot-with-gramjs-21ef5fb01822?source=collection_archive---------8----------------------->

![](img/41f9acc938815fff37f2a26873676fa8.png)

Photo by [Yuyeung Lau](https://unsplash.com/@yuyeunglau?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

关于如何制作电报机器人的文章不少。Telegram 还有一个优秀的 API，可以为 Telegram 创建一个客户端。让我们在 Node.js 中实现一个控制台应用程序，它将接收来自 chat 的图片，并熟悉面向客户端的库 GramJS 和 Telegram API。

# 准备

我们将使用 GramJS 库来处理 Telegram API。

让我们首先为我们的应用程序创建一个基本的包装器:

```
npm init -y && npm install telegram
```

这样，我们将创建一个新的模块，在这里我们将进行开发，同时，安装所需的库来使用 Telegram API。

# 批准

授权电报中的申请。我们希望以用户身份登录，而不是以机器人身份登录，因为机器人还有另一个 API，我们不需要它。

因为我们的应用程序并不大，所以我们不使用库来处理配置，而是将我们的秘密 apiID 和 apiHash 数据保存在 config.js 文件中，内容如下:

```
const config = {
  telegram_credentials: {
    apiId: <your apiId>,
    apiHash: <your apiHash>,
  },
  chatName: <your desired chat name>,
};
module.exports = config;
```

现在，我们使用示例中稍加修改的代码登录 Telegram。

```
const config = require('./config');
const {
  telegram_credentials: { apiId, apiHash },
  chatName,
} = config;
const stringSession = new StringSession("");
async function authorize() {
  const client = new TelegramClient(stringSession, apiId, apiHash, {
    connectionRetries: 5,
  });
  await client.start({
    phoneNumber: async () => await input.text('number ?'),
    password: async () => await input.text('password?'),
    phoneCode: async () => await input.text('Code ?'),
    onError: (err) => console.log(err),
  });
  console.log('You should now be connected.');
}
```

# 持久授权

为了避免每次登录，我们需要保存会话。我们将会话保存在同一个文件夹中，并在必要时进行检查和覆盖。我们已经在图书馆里得到了支持。是的，如果你为长期跑步实现了一个不同的机制，这将会有所帮助，但是对于我们的目的来说，它作为一个游戏场是很好的。

让我们重写授权代码以使用文件存储。

```
const storeSession = new StoreSession('my_session'); // This will save the session in a folder named my_session

async function authorize() {
  const client = new TelegramClient(storeSession, apiId, apiHash, {
    connectionRetries: 5,
  });
  // same function as above
}
```

为了检查授权，让我们细化我们的授权函数。让它首先尝试加入电报服务器，如果它不起作用，请再次引导我们通过授权流程。

为此，让我们添加以下代码，而不是立即开始授权

```
await client.connect();
if (await client.checkAuthorization()) { 
	// We are logged here
}
```

授权函数的完整代码将在本文末尾的清单中提供。

# 图像下载

出于演示目的，我们希望从配置中指定的聊天中获取图片的所有图片。为了下载图像，我们将使用 API 方法。为了避免多次下载相同的照片，让我们检查一下它们是否已经存在于我们电脑的文件夹中。

```
const fs = require('fs');
const path = require('path');

async function checkAndDownload(client, photo) {
  const filename = path.join('images', photo.id + '.jpg'); // Most of images in telegram in JPG
  try {
    await fs.promises.access(filename); // Check existance of file on the disk
    console.log(`File has already exist ${filename}`);
  } catch (error) {
    const buffer = await client.downloadMedia(photo, {
      progressCallback: console.log,
    });
    if (buffer !== undefined) {
      await fs.promises.writeFile(filename, buffer);
    }
  }
}
```

# 并行接收所有图像

如果图像很少，并且我们准备好立刻获取它们——就这么做吧。为此，使用必要的过滤器和聊天名称调用 getMessages 方法。

```
const images = await client.getMessages(chatName, {
    filter: new Api.InputMessagesFilterPhotos(),
  });
```

这种方法是合适的，因为它在一个请求中获得我们想要的所有消息，但是如果我们有超过 1000 条消息，它就不起作用了。

# 顺序接收图像

还有一种方法允许遍历所有消息。我们用它来得到所有的照片。

```
async function receiveMessages() {
  console.log('Start receive messages');

const client = await authorize();
  for await (const photo of client.iterMessages(chatName, {
    filter: new Api.InputMessagesFilterPhotos(),
  })) {
    try {
      await checkAndDownload(client, photo);
    } catch (error) {
      console.log(error);
    }
  }
  console.log('Finish download');
  return;
}
```

# 结论

我们创建了一个控制台应用程序，它可以从控制台接收参数，作为客户端连接到 Telegram，并保存和检索会话，这样您就不必每次都登录。与此同时，我们想出了如何避免电报服务器因不必要的请求而过载，并实现了一个处理大量图像的解决方案。我们获得了使用 Telegram API 和 GramJS 库的经验，允许我们在未来创建更复杂的应用程序。

# 资源

[https://gram.js.org/](https://gram.js.org/)

https://core.telegram.org/api

# 列表

```
const telegram = require('telegram');
const input = require('input');
const path = require('path');
const fs = require('fs');
const config = require('./config');

const {
    telegram_credentials: { apiId, apiHash },
    chatName,
} = config;
const storeSession = new telegram.sessions.StoreSession('my_session');
async function authorize() {
    const client = new telegram.TelegramClient(storeSession, apiId, apiHash, {
        connectionRetries: 5,
    });
    await client.connect();
    if (await client.checkAuthorization()) {
        console.log('I am logged in!');
    } else {
        console.log(
            "I am connected to telegram servers but not logged in with any account. Let's autorize"
        );
        await client.start({
            phoneNumber: async () => await input.text('number ?'),
            password: async () => await input.text('password?'),
            phoneCode: async () => await input.text('Code ?'),
            onError: (err) => console.log(err),
        });
        client.session.save();
        console.log('You should now be connected');
    }
    return client;
}
async function checkAndDownload(client, photo) {
    const filename = path.join('images', photo.id + '.jpg');
    try {
        await fs.promises.access(filename);
        console.log(`File has already exist  ${filename}`);
    } catch (error) {
        const buffer = await client.downloadMedia(photo, {
            progressCallback: console.log,
        });
        if (buffer !== undefined) {
            await fs.promises.writeFile(filename, buffer);
        }
    }
}
async function receiveMessages() {
    console.log('Start receive messages');
    const client = await authorize();
    for await (const photo of client.iterMessages(chatName, {
        filter: new telegram.Api.InputMessagesFilterPhotos(),
    })) {
        try {
            await checkAndDownload(client, photo);
        } catch (error) {
            console.log(error);
        }
    }
    console.log('Finish download');
    return;
}
(async () => {
    return await receiveMessages();
})();
```