# kot Lin——空安全调用以及为什么在使用它时要小心

> 原文：<https://medium.com/geekculture/kotlin-null-safe-calls-and-why-you-should-be-cautious-when-using-it-500cc362578a?source=collection_archive---------11----------------------->

*不仅仅是消除 IDE 中的错误*

![](img/68803a19f5c685ca98ba4dffbaf3c158.png)

Photo by [Mikey Harris](https://unsplash.com/@mikeyharris?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/home-office?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我现在已经在一家软件代理公司工作了一段时间，有 30 多个 android 开发者。在进行代码审查时，我无数次遇到了 Kotlin 的空安全调用的错误或“危险”用法，如下所示…

```
account?.let {
    imageView.loadImage(it.imageUrl)
}
```

很少有人关注这种方法…

## 这将隐藏错误

如果这个特定的方法或屏幕需要**帐户**对象，这样做将会隐藏错误并且很难调试。当**账户**对象在运行 app 时意外为空时，将不会向用户显示任何有用的信息。

避免这种情况的一种方法是当**帐户**对象为空时，优雅地处理。

```
account?.let {
    imageView.loadImage(it.imageUrl)
} ?: showErrorMessage("some helpful message here")
```

通过这种方式，最终用户可以清楚地看到应用程序出现了问题，而不仅仅是显示一个空白屏幕。

## 这使得调试变得困难

假设您正与两个或更多的开发人员一起开发这个特定的应用程序。当他们对类进行修改时，意外地发现**帐户**为空。其他开发人员将被晾在一边，不得不花时间调试代码，看看哪里出错了。

避免这种情况的一种方法是抛出一个异常，清楚地声明一条消息。

```
fun displayAccount(): {
    if (account == null) {
        throw IllegalStateException(
            "Account is unexpectedly null. Make sure to call init() before calling this method."
        )
    }
    ...
    // display account 
}
```

通过这种方式，您将使其他开发人员更容易看到是什么导致了错误。

# 结论

我们在使用 Kotlin 的空安全调用时需要小心，我们不能只是为了消除 IDE 中显示的错误而使用它们。

我并没有放弃使用科特林的无效安全呼叫的想法。其实这些都是很有用的，我们只需要知道什么时候适当的使用它们。

我能想到的一件事就是在 ViewModel 中清理资源。

```
// ViewModel.ktfun onCleared() {
   ...
   account?.removeListeners()
}
```

有可能**帐户**对象尚未初始化，用户立即关闭了屏幕——这时空安全调用就派上了用场。在这种情况下，没有必要显示用户友好的错误或抛出异常。

# 就是这样！

我希望我能通过这篇文章帮助一些人。感谢您的阅读和快乐编码！