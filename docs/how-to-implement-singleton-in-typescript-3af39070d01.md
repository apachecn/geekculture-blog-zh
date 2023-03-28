# 如何在 TypeScript 中实现 Singleton

> 原文：<https://medium.com/geekculture/how-to-implement-singleton-in-typescript-3af39070d01?source=collection_archive---------7----------------------->

![](img/07a8d292da80591b8fe1d3b70fa24378.png)

Photo by [Scott Webb](https://unsplash.com/@scottwebb?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

> 在软件工程中，单例模式是一种**软件设计模式，它将一个类的实例化限制为一个“单个”实例**。当只需要一个对象来协调整个系统的动作时，这很有用。这个术语来自独生子女的数学概念。

*免责声明:这篇文章不是要讨论单例设计模式是否真的有用，而是如何在 TypeScript 中实现它*

让我们来看看我们通常如何定义一个*类*

我相信你们大多数人都非常熟悉上面的模板，基本上我们有一个`constructor` 函数，它允许我们使用关键字`new`来创建一个新的实例。

现在，如果我需要使用这个定制的`log`函数，我可以通过声明一个新的`Logger` 类实例并调用该函数来轻松实现。但是从代码可读性的角度来看，拥有一个以上的`Logger`实例并没有什么意义，因为这个类不需要定制，这就是*单例设计模式*发挥作用的地方。

为了将上面的类变成一个*单例*类，首先我们需要禁用其他任何人来创建一个`Logger` 类的新实例，为了做到这一点，我们可以简单地向我们的构造函数添加一个`private` *访问修饰符*

从结果可以看出，当我们试图实例化该类时，TypeScript 立即对第 9 行感到不满。

然后，我们可以有一个类属性来存储单个实例，如下所示

```
private static instance: Logger;
```

请注意我们是如何将它标记为`private static`的，因此这是一个*静态*属性，不允许从类本身之外的任何地方访问，并且它的类型是`Logger` *。*

既然我们已经有了存储实例的类属性，我们只需要一个方法来创建并存储这个实例

```
static getInstance() {
  if (Logger.instance) {
    return this.instance;
  }
  this.instance = new Logger();
  return this.instance;
}
```

上面的代码非常简单，基本上我们检查我们之前定义的类属性`instance`是否有值，如果有，返回那个实例，否则我们通过`new`关键字创建一个新的实例。

你可能想知道为什么我们在这里可以有`new`关键字，这仅仅是因为`getInstance`方法本身在`Logger`类中，因此它可以访问构造函数——即使它被标记为`private`

就这样，我们已经将我们的`Logger`类转换成了一个 *Singleton。*下面是它的完整代码

感谢您的阅读，我希望这将帮助您了解更多的设计模式。为了获得反馈和合作机会，我邀请您与我联系，让我们继续对话！