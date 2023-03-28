# [Rust]Rust 和 RabbitMQ 使用 Crosstown 总线进行事件驱动编程[更新]

> 原文：<https://medium.com/geekculture/event-driven-programming-with-rust-and-rabbitmq-using-crosstown-bus-c39c50ce6c98?source=collection_archive---------4----------------------->

![](img/e1844e89a2b0cf0d4d5f6248c4d5413d.png)

Photo by [Jonathan Borba](https://unsplash.com/@jonathanborba?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/bus?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

事件驱动设计模式有助于我们创建可异步执行和独立伸缩的分布式应用程序。

当您在一个服务(例如一个 API)中处理数据，并且希望这个流程以异步方式触发一个不同的独立流程(或多个流程)时，您可以使用一个消息代理(例如 RabbitMQ)来触发它们。

[Crosstown Bus](https://crates.io/crates/crosstown_bus) 使使用 RabbitMQ 更容易，但提供了一个 SDK，您可以通过最少的配置连接到 RabbitMQ，允许您拥有多个发布者和多个订阅者，这又可以使用强类型对象订阅一个或多个事件。

点击查看[的跨城巴士文件。](https://docs.rs/crosstown_bus/0.2.1/crosstown_bus/)

为了更好地理解这一点，让我们先动手吧。

# 先决条件:

*   锈
*   VS 代码(或您最喜欢的 IDE /编辑器)
*   RabbitMQ —我使用 docker 在本地运行它

在您的终端中创建一个文件夹，紧接着，在其中创建两个 Rust 项目:

```
mkdir crosstown_poccd crosstown_poccargo new --bin publishercargo new --bin subscriber
```

# 订户

首先让我们创建订户。为此，请在订户文件夹中打开您的编辑器。

编辑 Cargo.toml 文件以添加 crosstown_bus 依赖项。Borsh 也是序列化和反序列化我们的对象所需要的。

```
crosstown_bus = "0.5.0"
borsh = "0.9.3"
borsh-derive = "0.9.1"
```

每次我们马上要写的发布者发布一条消息(在本例中是 CustomerCreatedEvent)时，处理函数将接收发布的消息。注意，这个事件结构必须派生出 *BorshSerialize* 和 *BorshDeserialize* 。

然后，让我们创建表示要发布的事件的结构。

```
#[derive(Debug, Clone, BorshDeserialize, BorshSerialize)]
pub struct UserCreatedEventMessage {
    pub user_id: String,
    pub user_name: String
}
```

我还添加了 *Debug* ，这样我们就可以根据需要打印对象了。

需要以下导入:

```
use std::error::Error;

use borsh::{BorshDeserialize, BorshSerialize};
use crosstown_bus::{MessageHandler, CrosstownBus, HandleError, QueueProperties};
```

现在让我们调用函数来订阅事件。为此，我们将:

1.  使用 **new_queue_subscriber** 方法创建一个订户对象。
2.  调用方法来侦听事件。

```
let listener = CrosstownBus::new_queue_listener("amqp://guest:guest@localhost:5672".to_owned()).unwrap();
_ = listener.listen("user_created".to_owned(), UserCreatedHandler{}, crosstown_bus::QueueProperties { auto_delete: false, durable: false, use_dead_letter: true });
```

请注意，处理程序实现必须作为参数传递。在这种情况下， *UserCreatedHandler* 将接收消息并处理它。它必须从 CrosstownBus 实现 MessageHandler。

我们示例中的处理程序将如下定义:

```
pub struct UserCreatedHandler;

impl MessageHandler<UserCreatedEventMessage> for UserCreatedHandler {
    fn handle(&self, message: Box<UserCreatedEventMessage>) -> Result<(), HandleError> {
        println!("Message received on handler 1: {:?}", message);
        Ok(())
    }
}
```

CrosstownBus 现在还支持 *QueueProperties* ，在这里您可以指定诸如自动删除和死信策略之类的配置。

订户主文件的完整代码:

```
use borsh::{BorshDeserialize, BorshSerialize};
use crosstown_bus::{CrosstownBus, MessageHandler, HandleError};

#[derive(Debug, Clone, BorshDeserialize, BorshSerialize)]
pub struct UserCreatedEventMessage {
    pub user_id: String,
    pub user_name: String
}

pub struct UserCreatedHandler;

impl MessageHandler<UserCreatedEventMessage> for UserCreatedHandler {
    fn handle(&self, message: Box<UserCreatedEventMessage>
    ) -> Result<(), HandleError> {
        println!("Message received: {:?}", message);
        Ok(())
    }
}

fn main() {
    let listener = CrosstownBus::new_queue_listener("amqp://guest:guest@localhost:5672".to_owned()).unwrap();
    _ = listener.listen("user_created".to_owned(), UserCreatedHandler{}, crosstown_bus::QueueProperties { auto_delete: false, durable: false, use_dead_letter: true });

    loop {
    }
}
```

# 出版者

首先，导航到先前创建的 Publisher 项目文件夹，并打开您的编辑器(最好是它的第二个窗口)。

将依赖项添加到。toml 文件。和我们在订户里用的一样。

现在，在 main.rs 文件中，让我们创建与在 Subscriber 项目中创建的结构相同的结构。这样，我们将发送和接收相同的类型。

```
 #[derive(Debug, Clone, BorshDeserialize, BorshSerialize)]
pub struct UserCreatedEventMessage {
    pub user_id: String,
    pub user_name: String
}
```

**旁注:**在现实世界的应用程序中，您可能会有一个包或其他机制来共享这个结构，而不必复制这个代码。

现在我们必须创建 publisher 对象。

之后，让我们调用 publish 方法来发布多个消息。

```
let mut p = CrosstownBus::new_queue_publisher("amqp://guest:guest@localhost:5672".to_owned()).unwrap();
    _ = p.publish_event("user_created".to_owned(), UserCreatedEventMessage { user_id: "5".to_owned(), user_name: "Zer0".to_owned() });
    _ = p.publish_event("user_created".to_owned(), UserCreatedEventMessage { user_id: "90".to_owned(), user_name: "Salvador".to_owned() });
    _ = p.publish_event("user_created".to_owned(), UserCreatedEventMessage { user_id: "8".to_owned(), user_name: "Maya".to_owned() });
    _ = p.publish_event("user_created".to_owned(), UserCreatedEventMessage { user_id: "7".to_owned(), user_name: "Rhys".to_owned() });
    _ = p.publish_event("user_createdx".to_owned(), UserCreatedEventMessage { user_id: "9".to_owned(), user_name: "Axton".to_owned() });
```

消息类型与侦听器预期的类型相同，事件名称也相同。

对于最后一条消息，请注意我输入了错误的事件名称。此消息将被发送到另一个 RabbitMQ 交换，我们的侦听器不会收到它。

这是我们出版商的最终代码:

```
use borsh::{BorshDeserialize, BorshSerialize};
use crosstown_bus::{CrosstownBus, MessageHandler, HandleError};

#[derive(Debug, Clone, BorshDeserialize, BorshSerialize)]
pub struct UserCreatedEventMessage {
    pub user_id: String,
    pub user_name: String
}

pub struct UserCreatedHandler;

impl MessageHandler<UserCreatedEventMessage> for UserCreatedHandler {
    fn handle(&self, message: Box<UserCreatedEventMessage>) -> Result<(), HandleError> {
        println!("Message received on handler 1: {:?}", message);
        Ok(())
    }
}

fn main() {
    let mut p = CrosstownBus::new_queue_publisher("amqp://guest:guest@localhost:5672".to_owned()).unwrap();
    _ = p.publish_event("user_created".to_owned(), UserCreatedEventMessage { user_id: "5".to_owned(), user_name: "Zer0".to_owned() });
    _ = p.publish_event("user_created".to_owned(), UserCreatedEventMessage { user_id: "90".to_owned(), user_name: "Salvador".to_owned() });
    _ = p.publish_event("user_created".to_owned(), UserCreatedEventMessage { user_id: "8".to_owned(), user_name: "Maya".to_owned() });
    _ = p.publish_event("user_created".to_owned(), UserCreatedEventMessage { user_id: "7".to_owned(), user_name: "Rhys".to_owned() });
    _ = p.publish_event("user_createdx".to_owned(), UserCreatedEventMessage { user_id: "9".to_owned(), user_name: "Axton".to_owned() });
}
```

# 测试我们的应用

为此，我们需要:

1.  启动 RabbiMQ
2.  启动订户
3.  启动 Publisher

## 启动 RabbitMQ

在我的例子中，我在[码头](https://hub.docker.com/_/rabbitmq)上运行它。

在浏览器上打开 RabbitMQ Admin。我的运行在本地主机:15672 上。

![](img/3462883d26d557ff5d01da4fb9efa6e8.png)

RabbitMQ Admin

## 启动订户

导航到订户文件夹并运行以下命令:

```
cargo run
```

![](img/7f0bd2a45fb79e01d98ef9ecd1229d7c.png)

Subscriber program being executed

## 启动 Publisher

在不同的命令/终端窗口中，运行 publisher 程序文件夹中的 *cargo run* 。

![](img/5b4c7f1a6186fe7d44aad47e3d3f1ce2.png)

Publisher executed

## 检查结果

返回到 publisher 终端窗口。

```
 Running `target/debug/subscriber`
Message received: UserCreatedEventMessage { user_id: "5", user_name: "Zer0" }
Message received: UserCreatedEventMessage { user_id: "90", user_name: "Salvador" }
Message received: UserCreatedEventMessage { user_id: "8", user_name: "Maya" }
Message received: UserCreatedEventMessage { user_id: "7", user_name: "Rhys" }
```

看看吧！两个事件都由两个订阅者发送和接收。

而这就是全部！

完整代码请看我的 GitHub:[GitHub—paoloposso/cross town _ bus _ client _ POC](https://github.com/paoloposso/crosstown_bus_client_poc)。

谢谢，下次再见！