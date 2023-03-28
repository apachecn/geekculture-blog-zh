# 360 IT 检查#54 —新 iPhone，在两个新国家的 Windows 上运行 Android 应用程序，等等！

> 原文：<https://medium.com/geekculture/360-it-check-54-new-iphone-running-android-apps-on-windows-in-two-new-countries-and-more-6c0bf6bfa096?source=collection_archive---------19----------------------->

![](img/3da23015554d7e14dc8501e775f3ced6.png)

# 新款 iPhone

每一款新 iPhone 的发布都是一个事件[所有发布 iOS 应用的公司都必须紧跟](https://www.itmagination.com/services/custom-software-development/mobile-application-development)。苹果手机在一些关键的西方市场处于领先地位——比如美国、加拿大和英国(在英国，iOS 和 Android 的市场份额非常接近)。

今年，我们看到了自 iPhone X 以来 iPhone 的最大更新。顶级型号 iPhone Pro 的特点是小得多的凹槽，内置相同的红外传感器。这是对上一款车型的稳步发展，在上一款车型中，这家加州公司缩小了剪裁尺寸，但并未取消剪裁。

至于新功能，列表如下(非详尽):

*   蓝牙 5.3
*   动态岛(通知有了新的外观)
*   更强大的电池
*   始终展示
*   光子引擎

奇怪的是，这家美国手机制造商决定不使用 USB-C 端口进行充电和数据传输。

**底线**

新 iPhone 很无聊。什么都没有从根本上改变；没什么奇怪的。不过，缺口消失了，这是最大的优势。被大肆宣传的动态岛无非是增加了一种新的、奇特的显示通知的方式。“始终展示”是一个很好的功能，但是竞争对手已经拥有这个功能将近 10 年了。

然而，iOS 开发者可能会以不同的眼光看待这款新推出的手机，所以我们有来自 ITMAGINATION 的资深 iOS 开发者 T4【Andrej Krizmanic 的评论:

> 不要指望新推出的 iPhones 会有什么革命性的东西。四个型号获得了增量更新，阵容得到了刷新。在我看来，苹果正在强调完整的产品线，而不仅仅是旗舰机型。因此，现在，美国的客户可以获得一款非常强大的全新 iPhone 机型，起价为 429 美元(iPhone SE，基本款)，最大的旗舰机型(iPhone Pro Max，基本款)价格高达 1099 美元。在这里，我想指出的是，SE 型号将满足我母亲 4 年甚至更长时间的需求。
> 
> *因此，作为开发者，我们得到的是更广泛的用户，他们手中握有非常强大的设备。这些用户正在将他们的体验扩展到手机之外，更多的人为他们的 Apple Watch、Apple TV 找到了一个很好的使用案例，一些人转向 MAC，一些人更频繁地使用他们的 iPad。有必要为所有设备类型的用户开发体验。*

# 微软将 Android 功能的 Windows 子系统扩展到新的地区

微软已经在更多地区推出了兼容性功能，并进行了改进。该功能允许在亚马逊应用商店的帮助下在 Windows 上运行 Android 应用。这个不太可能的联盟目前只在美国可用，尽管它的可用性刚刚扩展到日本和德国。

底线

Windows 新功能的推出，被一些人吹捧为至关重要的一项，至少一直进展缓慢。尽管从技术上讲，它在所有地区都可以工作，但安装它并不容易。对许多人来说，这无疑是一个巨大的损失，因为它允许用户扩展他们电脑的功能:有时应用程序只能在手机上使用。

# 预动作信号

[Preact，快速而小巧的 react 替代产品，已经宣布发布一个有趣的功能。](https://preactjs.com/blog/introducing-signals/) Signals 是该框架的新状态管理方法，专注于创建反应状态的片段(简单地说，数据将重新呈现一次，并且只改变一次)。

这个 API 非常简单，您可以通过编写*' import { signal } from @ preact/signals '*从今天开始使用这个新特性:

```
*import {signal} from @preact/signals*const count = signal(0);function Counter() { 
return (  
        <button onClick={() => count.value++}>
            Value: {count}
        </button>  
    ); 
}
```

**底线**

这与 React 形成了鲜明的对比。脸书的库旨在只提供最基本的功能，只专注于在屏幕上呈现。状态管理和所有其他问题都留给了开发人员。

当然，尤雨溪很快指出 Preact 背离了 react 的哲学。

事实上，它确实向 Vue 靠拢了；更倾向于不仅仅包含最低限度的模型。我们不是在谈论包的生态系统，比如 Angular 的:只是对视图层的有益补充。

[这里是黑客新闻线程，供所有感兴趣的人参考。](https://news.ycombinator.com/item?id=32743078)

# 奖金

# 10 个强大的 AI/ML API

[https://nordicapis.com/10-powerful-ai-ml-apis/](https://nordicapis.com/10-powerful-ai-ml-apis/)

# enhance . dev——一个新的、HTML 优先的 Web 框架

[https://bytes.dev/archives/116](https://bytes.dev/archives/116)

‍

*最初发表于*[*【https://www.itmagination.com】*](https://www.itmagination.com/blog/360deg-it-check-54-new-iphone-14-android-windows-wsla-preact-ml-ai)*。*