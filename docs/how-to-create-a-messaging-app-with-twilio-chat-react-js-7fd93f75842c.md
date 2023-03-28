# 如何使用 Twilio Chat & React JS 创建消息应用程序

> 原文：<https://medium.com/geekculture/how-to-create-a-messaging-app-with-twilio-chat-react-js-7fd93f75842c?source=collection_archive---------4----------------------->

![](img/24297db9b847040c989c75e6b66da357.png)

# 让我们使用 Twilio 的聊天 API 来构建一个多用户消息应用程序。

*在这篇文章中，学习如何实现 Twilio 对话 API 来创建一个基本的聊天室应用程序。你可以用这些方法创建一个类似 Slack* 的 app

# 您需要什么:

*   Twilio 免费试用
*   [Twilio 聊天 API](https://www.twilio.com/console/chat/services)
*   初级反应知识
*   NodeJS(和首选的包管理器，我将使用 yarn)

# 我们开始吧

## 首先，让我们用 create-react-app 设置我们的应用程序。

继续创建您的项目目录。我通常会进入我所有项目的文件夹，找到与我正在处理的内容相关的特定文件夹，因此对于这个示例，我将位于我的 React 文件夹中，在那里我保存了使用 React 创建的所有项目。

*例如桌面/项目/反应/反应聊天*

然后，我将创建我的文件夹，右键单击并在代码中打开。

您也可以打开终端， *mkdir dirNam* e 并运行*。代码*

但是我喜欢在 VSCode 上使用内置终端。

现在运行以下命令:

```
npx create-react-app frontend
cd frontend
yarn add axios react-router-dom twilio-chat @material-ui/core
```

*或 npm 安装，具体取决于您的软件包管理器首选项*

## 后端设置

前端大部分都设置好了，让我们继续设置我们的后端。

让我们导航回主目录。

```
cd ..
```

在 reactjs-chat(或您的主目录)中，让我们运行以下代码:

```
git clone [https://github.com/TwilioDevEd/sdk-starter-node.git](https://github.com/TwilioDevEd/sdk-starter-node.git)
cd sdk-starter-node
yarn install 
yarn add cors
```

基本上，我们想要克隆 repo Twilio 为他们的服务创建的一个简单的开始，这使我们能够访问通知处理和令牌生成等内容。令牌生成是我们需要它的原因。

接下来，导航到 VSCode 上的 app.js 并找到以下代码行:

```
const app = express();
```

复制以下代码，并将其直接粘贴到上面一行的下方。

```
const cors = require('cors');
app.use(cors());
```

这代表**跨产地资源共享。**

它允许服务器指示除了它自己以外的任何其它来源，浏览器应该允许从这些来源加载资源。基本上，我们需要它来访问外部 API。

## Twilio Keys

如果你还没有 Twilio 帐户，请到这里免费试用。

在您的 **sdk-start-node** 目录中，您应该会看到一个名为`.env.example`的文件，我们将其重命名为`.env`

它将有一个预填充的例子，为您需要获得的所有密钥。

第一个在你的**账户仪表盘上**，[这里](https://www.twilio.com/console)，你会看到**账户 SID** ，把它复制到`TWILIO_ACCOUNT_SID`里。环境文件。

接下来，您需要一个 API 密钥来进行身份验证。转到工具条中的**设置> API 密钥**并点击**创建 API 密钥。**

复制 **SID** 和 **SECRET** 并将它们作为`TWILIO_API_KEY`和`TWILIO_API_SECRET`的值粘贴到您的`.env`中

至此，我们已经完成了一般的帐户设置。我们需要去[这里](https://www.twilio.com/console/chat/services)

创建聊天服务。您需要单击加号按钮并命名您的服务。

为您的服务复制**服务 SID** ，并将其粘贴为`TWILIO_CHAT_SERVICE_SID`环境变量的值。

现在，我们可以在 sdk-starter-node 目录中运行以下内容:

```
yarn start
```

服务器将从`localhost:3000`开始，看起来像这样:

## “登录”和聊天屏幕

我们的后端已经完成，所以让我们的应用程序功能和外观漂亮。

我为这个应用程序创建了一个非常简单和基本的前端，你可以随心所欲地设计你的风格。

这个想法将是首先提示用户一个电子邮件，然后让用户选择一个聊天室。

让我们回到前端目录，找到您的 index.js 并将<app>组件包装在路由器中。</app>

```
ReactDOM.render(
  <React.StrictMode>
    <Router>
      <App />
    </Router>
  </React.StrictMode>,
  document.getElementById('root')
);
​
```

当然，不要忘记导入路由器

```
import { BrowserRouter as Router } from 'react-router-dom'
```

这就是你需要对 index.js 做的所有事情，继续并关闭它。

转到 App.js，删除主容器和 App 的类名之间的所有内容。移除内容后，将其包在一条路线中并进行切换。

下面我将我的应用程序 div 包装在一个默认路径中，并添加了我将用于聊天屏幕的路径。

```
return (
    <Switch>
      <Route path="/:room" component={Chat} />
      <Route path="/">
        <div className="App">

      	</div>
      </Route>
    </Switch>
  );
```

您的导入应该如下所示:

```
import './App.css';
import React, { useState } from "react";
import { Route, Switch, useHistory } from "react-router-dom"; 
import Chat from './components/Chat';
```

我们将使用自定义钩子进行状态维护。

在您的 **src** 目录中，创建一个名为 **components** 的文件夹。然后创建一个名为 Chat.js 的文件，并立即输入`_rfce`。(你需要[这个](https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets)插件，否则只需输入你的函数)

```
function Chat() {

    return (
        <div className="chatScreen">

        </div>
    )
}export default Chat
```

好了，让我们回到 App.js 并获取用户的电子邮件。

这是我的布局:

```
return (
    <Switch>
      <Route path="/:room" component={Chat} />
      <Route path="/">
        <div className="container">
        <main className="main">
          <h1 className="title">
            Welcome
          </h1> <section className="loginSection">
            <input type="email" onChange={(e)=>updateEmail(e.target.value)} placeholder="Email" />
            {
              emailWarning&&
              <span className="warning">You need to enter your email.</span>
            }
            <button onClick={login}>Continue</button>
          </section>

        </main>
      </div>
      </Route>
    </Switch>
  );
```

我正在用一个自定义钩子更新 email 变量；我们马上就知道了。首先让我们设计这些元素的样式:

```
.container {
  min-height: 100vh;
  padding: 0 0.5rem;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}.main {
  padding: 5rem 0;
  flex: 1;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  max-height: 100vh;
  overflow: hidden;
}.loginSection{
  margin-top: 20px;
  height: 70vh;
  width: 70vw; display: flex;
  flex-direction: column;
  align-items: center;
}
.loginSection input{
  margin-top: 30px;
  width: 500px;
  padding: 10px;
  border: 2px solid transparent;
  outline: none;
  border-radius: 10px;
}
.loginSection input:focus{
  border: 2px solid grey;
}
.loginSection button{
  margin-top: 30px;
  width: 500px;
  padding: 15px;
  background-color: rgba(74, 146, 74, 0.4);
  border: 2px solid rgba(103, 175, 103, 0.4);
  outline: none;
  color: white;
  border-radius: 10px;
  cursor: pointer;
}
.loginSection button:hover{
  background-color: rgb(74, 146, 74);
  border: 2px solid rgb(103, 175, 103);
}.warning{
  color: rgb(211, 63, 63);
}
```

一旦我们开始工作，我们将有一个非常基本的欢迎屏幕:

接下来让我们创建钩子。我还将电子邮件保存在 localStorage 中，我们将使用 react-router-dom 的 useHistory 钩子将页面推送到聊天屏幕。

当用户键入时，每个变化将运行`updateEmail()`这运行我们的定制钩子:`setEmail()`。它还将 emailWarning 设置为 false，以防在用户没有首先输入电子邮件就点击 continue 时它被更改为 true。

这里没有电子邮件验证，但是如果你愿意，你可以很容易地实现它。

```
const [email, setEmail] = useState('')
  const [emailWarning, setEmailWarning] = useState(false);

  let history = useHistory();
  function login() {
    if (email) {
      localStorage.setItem('email', email);
      history.push("chat");
    }
    else if(!email){
      setEmailWarning(true)
    }
  } const updateEmail = (e) => {
    setEmail(e)
    setEmailWarning(false)
  }
```

一旦我们有了用户的电子邮件，我们就将我们的域推送到聊天屏幕，这样 App.js 就完成了，让我们来构建聊天屏幕。

## 聊天屏幕

首先，这是我们的进口货

```
import { useHistory } from "react-router-dom"; 
import axios from "axios";
import ChatItem from "./ChatItem";
import React, { useRef } from "react";
import { useEffect, useState } from "react";
const ChatAPI = require("twilio-chat");
```

我们将使用 **axios** 从我们自己的后端获取令牌，并使用 **useRef** 访问聊天内容，将它们滚动到底部。

继续在组件目录中创建 ChatItem.js。

以下是我的 Chat.js 回复:

```
return (
        <div className="chatScreen">
            <div className="sidebar">
                <h4>{email}</h4>
                <h2>Rooms</h2>
                {
                    roomsList.map((room) =>(
                        <p key={room} onClick={()=>changeRoom(room)}>	{room}</p>
                    ))
                }
            </div> <div className="chatContainer" ref={scrollDiv}>
                <div className="chatHeader">
                    {room === "chat" ? "Choose A Room" : room}
                </div> <div className="chatContents"> {(messages && room !== "chat") &&
                messages.map((message) => 
                  <ChatItem
                    key={message.index}
                    message={message}
                    email={email}/>
                )} </div> {
                    room !== "chat" &&
                <div className="chatFooter">
                    <input type="text" placeholder="Type Message" onChange={(e)=>updateText(e.target.value)} value={text} />
                    <button onClick={sendMessage} >Send</button>
                </div>
                } </div>
        </div>
    )
```

我有一个侧边栏，让所有的房间都在边上，比如 Slack。点击一个将重新路由到每个房间的动态 URI。聊天容器有一个标题部分，显示您所在的房间名称，底部是输入字段。

继续定义所有的常量和自定义挂钩:

```
const email = localStorage.getItem('email');
    const room = window.location.pathname.split('/')[1]; const [loading, setLoading] = useState(false);
    const [messages, setMessages] = useState([]);
    const [channel, setChannel] = useState(null);
    const [text, setText] = useState("");

    const roomsList = ["general"];
    let scrollDiv = useRef(null);
```

我们从动态路径中收集房间名，并去掉正斜杠。电子邮件在本地存储中，因此我们可以从那里轻松访问它。

然后，我们创建用于加载的钩子、消息列表、通道(由 Twilio 创建)和文本(用户输入)。房间列表可用于本地目的，但您也可以将它存储在数据库中，以便以更动态的方式创建更多房间。

最后一个变量 scrollDiv 将创建一个我们已经绑定到 chatContainer div 的引用。

我们侧边栏中的每个房间都有一个 onClick 事件来改变房间，功能很简单:

```
let history = useHistory();
const changeRoom = room => history.push(room);
```

让我们也在 onChange 上更新用户输入:

```
const updateText = e => setText(e);
```

这些都是简单的方法。真正重要的部分是得到一个令牌:

```
const getToken = async (email) => {
        const response = await axios.get(`http://localhost:3000/token/${email}`);
        const { data } = response;
        return data.token;
      }
```

这将访问我们的后端 api，它基本上是由 Twilio 提供的，并返回一个令牌。

接下来，我们将构建其余的函数。页面加载后，`joinChannel`将接收我们创建的一个通道，我们将在一个`useEffect`钩子中添加。

在我们的 JavaScript 中，我们会看到类似`messageAdded`的通道监听器。这些是由 Twilio 提供的。下面我们还有`handleMessageAdded`，它运行在前面提到的监听器上，获取与您当前房间相关的返回消息，并将它们添加到我们的消息数组中。

然后，我们创建我们的`sendMessage`函数，当用户点击发送时，这个函数被调用。

```
const joinChannel = async (channel) => {
        if (channel.channelState.status !== "joined") {
         await channel.join();
       }

       setChannel(channel);
       setLoading(false)

       channel.on('messageAdded', function(message) {
        handleMessageAdded(message)
      });
    	scrollToBottom();
     };
const handleMessageAdded = message => {
        setMessages(messages =>[...messages, message]);
        scrollToBottom();
      };

      const scrollToBottom = () => {
        const scrollHeight = scrollDiv.current.scrollHeight;
        const height = scrollDiv.current.clientHeight;
        const maxScrollTop = scrollHeight - height;
        scrollDiv.current.scrollTop = maxScrollTop > 0 ? maxScrollTop : 0;
      }; const sendMessage = () => {
        if (text) {
            console.log(String(text).trim())
            setLoading(true)
            channel.sendMessage(String(text).trim());
            setText('');
            setLoading(false)
        }
      };
```

我们的 Chat.js 的最后一部分是 useEffect 钩子，它将在页面加载完成时触发频道创建:

```
useEffect(async() => {
        let token = ""; if (!email) {
            history.push("/");
        } setLoading(true) try {
          token = await getToken(email);
        } catch {
          throw new Error("Unable to get token, please reload this page");
        } const client = await ChatAPI.Client.create(token); client.on("tokenAboutToExpire", async () => {
            const token = await getToken(email);
            client.updateToken(token);
        }); client.on("tokenExpired", async () => {
            const token = await getToken(email);
            client.updateToken(token);
        }); client.on("channelJoined", async (channel) => {
            const newMessages = await channel.getMessages();
            console.log(newMessages)
            setMessages(newMessages.items || []);
            scrollToBottom();
          });

          try {
            const channel = await client.getChannelByUniqueName(room);
              console.log(channel)
              joinChannel(channel);
              setChannel(channel)
          } catch(err) {
            try {
              const channel = await client.createChannel({
                uniqueName: room,
                friendlyName: room,
              });

              joinChannel(channel);
            } catch {
              throw new Error("Unable to create channel, please reload this page");
            }
          }    }, [])
```

首先，我们确保有一封电子邮件，如果没有，我们会把用户送回欢迎页面。然后，我们使用电子邮件来获取一个令牌，我们使用这个令牌来创建 ChatAPI 客户端。然后，客户端被用来创建一个存储大量信息的通道，比如附加到所述通道或房间的消息对象。消息对象包含它们自己的创建日期、作者等。

在我们的 JSX 中，我们将消息对象传递到 ChatItem 组件中，从那里，我们可以使用存储在每个对象中的信息来创建我们的聊天气泡。

让我们来看看 ChatItem.js。

我们的进口:

```
import React from "react";
import { ListItem } from "@material-ui/core";
```

这里有一些可以使用的样式，我们将把它们添加到 react 文件中，而不是 css 文件中，这样我们就可以轻松地为用户响应和传入响应创建动态样式，而无需创建多个类。两个选项都有效。

```
const styles = {
  listItem: (userMsg) => ({
    flexDirection: "column",
    alignItems: userMsg ? "flex-end" : "flex-start",
  }),
  container: (userMsg) => ({
    maxWidth: "75%",
    borderRadius: 10,
    padding: 10,
    color: "white",
    fontSize: 12,
    backgroundColor: userMsg ? "#F36E65" : "#9ea1a8",
  }),
  author: { fontSize: 10, color: "gray" },
  timestamp: { fontSize: 8, color: "white", textAlign: "right", paddingTop: 5 },
};
```

我们只需要几个常量，我们从道具中得到它们:

```
function ChatItem(props) {

    const message = props.message;
    const email = props.email;
    const userMsg = message.author === email;
```

最后，JSX:

```
return (
      <ListItem style={styles.listItem(userMsg)}>
        <div style={styles.author}>{message.author}</div>
        <div style={styles.container(userMsg)}>
          {message.body}
          <div>
            {new Date(message.dateCreated.toISOString()).toLocaleString()}
          </div>
        </div>
      </ListItem>
    );
```

此时，您应该能够运行`yarn start`并使用您的电子邮件登录。

# 结论

我复习了实现 Twilio API 的所有基础知识，得到了一个可以工作的消息应用程序。我希望它对你有所帮助，你可以随时查阅这些文档以获得更深入的信息，[https://www.twilio.com/docs/chat](https://www.twilio.com/docs/chat)。

[项目回购](https://github.com/tannerkc/TwilioReactChatApp)