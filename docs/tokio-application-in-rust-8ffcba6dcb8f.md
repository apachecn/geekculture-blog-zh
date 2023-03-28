# Tokio 在 Rust 中的应用

> 原文：<https://medium.com/geekculture/tokio-application-in-rust-8ffcba6dcb8f?source=collection_archive---------8----------------------->

![](img/ba5f78d176acf73c1c18271a874d7368.png)

# 介绍

rust 中的 Tokio 应用是一个异步运行时和网络应用框架。这是为 Rust 编程语言中客户机和服务器的快速开发和高可伸缩性部署而开发的。它提供了编写网络应用程序所需的构件。它提供了针对各种系统的灵活性。这些系统从具有几十个内核的大型服务器到小型嵌入式设备都有。

在本文中，我们将了解 [Rust 编程语言](https://www.technologiesinindustry4.com/2021/07/rust-refactoring-to-enhance-modularity.html)中的 Tokio 应用。

# 描述

Tokio application 是一个事件驱动的平台，用 [Rust 编程语言](https://www.technologiesinindustry4.com/2021/07/rust-refactoring-to-enhance-modularity.html)编写异步应用。它给出了几个高层次的主要组件:

*   用于实现异步代码的多线程运行时。
*   标准库的异步版本。
*   一个更大的图书馆生态系统。

# Tokio 的优势

# 快速的

*   Tokio 是快速的，建立在 [Rust 编程语言](https://www.technologiesinindustry4.com/2021/07/rust-refactoring-to-enhance-modularity.html)之上。
*   这是本着 Rust 的精神设计的，目标是我们不应该通过手工编写等价代码来提高效率。
*   Tokio 是可扩展的，建立在 async 或 await 语言特性之上。
*   由于处理网络时的延迟，我们处理连接的速度是有限的。
*   因此，唯一的扩展方向是一次控制多个连接。
*   利用 async 或 await 语言特性，增加并发操作的数量变得非常便宜。
*   这使我们能够扩展到大量并发任务。

# 信任

*   Tokio 应用程序是使用 [Rust](https://www.technologiesinindustry4.com/2021/07/rust-refactoring-to-enhance-modularity.html) 构建的。
*   Rust 是一种让每个人都能构建可信且高效的软件的语言。
*   在不同的研究中发现，大约 70%的高严重性安全错误是内存不安全的结果。
*   通过使用[, Rust](https://www.technologiesinindustry4.com/2021/07/rust-refactoring-to-enhance-modularity.html)移除了应用程序中的这一整类错误。
*   Tokio 同样强调给予一致的行为，没有惊喜。
*   Tokio 的主要目标是允许用户部署可预测的软件。
*   这将日复一日地做同样的事情，具有可靠的响应时间，并且没有不可预测的延迟峰值。

# 简单易行

*   Rust 的 async 或 await 特性大大降低了编写异步应用程序的复杂性。
*   利用 Tokio 的实用程序和充满活力的生态系统，编写应用程序很容易。
*   当有意义时，Tokyo 应用程序采用标准库的命名约定。
*   这使得很容易将只用标准库编写的代码转换成用 Tokyo 编写的代码。
*   轻松提供正确代码的能力是强类型系统 [Rust](https://www.technologiesinindustry4.com/2021/07/rust-refactoring-to-enhance-modularity.html) 无法比拟的。

# 容易修改

*   Tokio 应用程序给出了运行时的不同变化。
*   所有事情都来自多线程
*   轻量级的工作窃取运行时
*   单线程运行时

这些运行时中的每一个都带有多个旋钮，使用户能够根据自己的要求进行调整。

# 服务特性

服务特质是 Tokio 的基石。通过标准化客户机和服务器使用的接口，它使得编写可组合和可重用的组件成为可能。

```
pub trait Service: Send + 'static {
    type Req: Send + 'static; type Resp: Send + 'static; type Error: Send + 'static; type Fut: Future<Item = Self::Resp, Error = Self::Error>; fn call(&self, req: Self::Req) -> Self::Fut;
}
```

*   一个简单的特征，尽管它开启了一个可能性的世界。
*   这种对称和统一的 API 将服务器或客户机简化为一种功能。
*   因此，它允许中间件或过滤器，这取决于行话。
*   服务是从请求到响应的异步函数。
*   异步性由期货箱管理。
*   中间件这个组件同时充当客户机和服务器。
*   它拦截请求，修改请求，并将其传递给上游服务值。

**以超时为例**

```
pub struct Timeout<T> {
    upstream: T,
    delay: Duration,
    timer: Timer,
}impl<T> Timeout<T> {
    pub fn new(upstream: T, delay: Duration) -> Timeout<T> {
        Timeout {
            upstream: upstream,
            delay: delay,
            timer: Timer::default(),
        }
    }
}impl<T> Service for Timeout<T>
    where T: Service,
          T::Error: From<Expired>,
{
    type Req = T::Req;
    type Resp = T::Resp;
    type Error = T::Error;
    type Fut = Box<Future<Item = Self::Resp, Error = Self::Error>>; fn call(&self, req: Self::Req) -> Self::Fut {
        // Get a future representing the timeout and map it to the Service error type
        let timeout = self.timer.timeout(self.delay)
            .and_then(|timeout| Err(Self::Error::from(timeout))); // Call the upstream service, passing along the request
        self.upstream.call(req)
            // Wait for either the upstream response or the timeout to happen
            .select(timeout)
            // Map the result of the select back to the expected Response type
            .map(|(v, _)| v)
            .map_err(|(e, _)| e)
            .boxed()
    }
}
```

*   上面显示的超时中间件只需编写一次。
*   那么它可以作为任何协议的任何网络客户机的一部分被插入，如下所示。

```
let client = Timeout::new(
    http::Client::new(),
    Duration::from_secs(60));client.call(http::Request::get("https://www.rust-lang.org"))
    .and_then(|response| {
        // Process the response
        Ok(())
    }).forget(); // Process the future
```

*   此后，有趣的事情是，真正相同的中间件也可以用在服务器上，见下文。

```
http::Server::new()
    .serve(Timeout::new(MyService, Duration::from_secs(60)));
```

# 在以下情况下不要使用 Tokio:

*   通过在不同的线程上并行运行 CPU 限制的计算来加速它们。
*   如果应用程序只做并行计算，我们应该使用 rayon。
*   读取一些文件。
*   与普通的线程池相比，Tokio 在这里没有提供任何好处。
*   这是因为操作系统通常不提供异步文件 API。
*   发送单个 web 请求。
*   当我们需要同时做许多事情时，Tokio 为我们提供了一个好处。
*   当我们需要使用像 reqwest 这样的用于异步 [Rust](https://www.technologiesinindustry4.com/2021/07/rust-refactoring-to-enhance-modularity.html) 的库时，我们应该更喜欢那个库的阻塞版本。
*   我们不需要一次做很多事情，因为这会使项目更简单。

更多详情请访问:[https://www.technologiesinindustry4.com](https://www.technologiesinindustry4.com)