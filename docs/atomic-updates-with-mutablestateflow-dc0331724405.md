# 可变状态流上的原子更新

> 原文：<https://medium.com/geekculture/atomic-updates-with-mutablestateflow-dc0331724405?source=collection_archive---------0----------------------->

![](img/63ecef2ed2318c980063e74a98723503.png)

Photo by [Guillermo Velarde](https://unsplash.com/@guille_velard?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

Kotlin 协同例程的 1.5.1 版本带来了有趣的[扩展函数](https://github.com/Kotlin/kotlinx.coroutines/issues/2720)，帮助对可变状态流的值进行“原子”更新。

# 一些背景

我将在 Android 的背景下描述这个问题，因为这是我最常使用的平台。但这个问题是 100% Kotlin，所以它可以很容易地适用于其他平台。

`StateFlow`通常用于在 Android 中常用的 MVVM 模式中保持和发出 UI 状态。例如，可能有一个`ViewModel`公开了一个数据类的`StateFlow`来描述视图状态。视图状态可以描述为一个数据类。

```
class MyViewModel : ViewModel() { data class ViewState(
     val showLoading: Boolean = false,
     val title: String = "Default Title",
     val doneButtonEnabled: Boolean = true
  ) private val _viewState = MutableStateFlow<ViewState>(ViewState())
  val viewState = _viewState.asStateFlow()}
```

活动或片段可以消费所述流，并使用它发出的值来改变 UI 状态。注意，我不打算讨论如何安全地观察 android 生命周期中的流程，这不是本文的重点。Manuel Vivo 的文章很好地涵盖了这一点。(无耻地为我自己的插上一句话，那是一篇覆盖类似主题的稍显陈旧的文章。)

```
class MyFragment : Fragment(R.layout.*fragment*) {
... // Note I'm ignoring details about where the flows are observed 
  // There are some good reasons to be careful how your flows
  // are observed here, but since that isn't the focus of the
  // article I'm omitting them.
  viewModel.viewState
    .onEach {
        if (it.showLoading) {
          // update the UI to show loading 
        }
        // and so on with the other parts of the view state
    }
    .launchIn... ...}
```

当与 StateFlow 结合使用时，使用数据类来描述 UI 状态是一种非常方便的机制。您可以反复观察流程，并始终获得最新的 UI 状态。观察值是一个普通的数据类，可以非常简单地分解成它的组件属性。(我要指出的是，使用一个密封的类作为你的 UI 状态可以表现出与本文中描述的相同的行为，只是演示起来更加复杂。)

此外，使用数据类更新状态很容易。数据类有一个方便的`copy`函数，允许您更新数据类的一个或多个属性，同时保留其余的值。所以您可以非常简单地更新 UI 状态:

```
// Update just one property, leave the rest as is without 
// caring what the values are.
val newUIState = _viewState.value.copy(doneButtonEnabled = true)
_viewState.value = newUIState // emit the new UI state
```

或者更好的是，只使用一个命令行:

```
_viewState.value = _viewState.value.copy(doneButtonEnabled = true)
```

这看起来非常简单明了。无论在 UI 状态中设置了什么其他属性，只有`doneButtonEnabled`属性被更新；剩余的属性将被保留。

# 问题是

当然，有一个问题。虽然代码都在一行中，但是需要注意一个竞争条件。在时间`copy`函数完成和状态流的新值发出之间，另一个线程可能已经更新了视图状态并改变了当前`copy`没有修改的属性之一。

考虑下面这个带有两个“并发”操作的非常人为的例子:

```
*// Assume the state flow value has the following existing properties:
// title: "Default title"
// doneButtonEnabled: false

// Label: Launch A
viewModelScope*.*launch*(Dispatchers.IO) **{** _viewState.value = _viewState.value.copy(doneButtonEnabled = true)
**}** *// Label: Launch B
viewModelScope*.*launch*(Dispatchers.Default) **{** _viewState.value = _viewState.value.copy(title = "New Title")
**}**
```

如果您在两个启动 lambdas 都完成后才开始观察状态流，您希望该值包含什么？

许多人期望最终发出的数据类包含一个`New Title`的`title`和一个`true`的`doneButtonEnabled`值。对于该代码块的许多执行来说，这可能是真的，但鉴于这两个 lambdas 的并发性质，并不是所有的执行都是真的。

事实是，一旦我们取消一些内联变量，上面的例子实际上就是下面的例子:

```
*// Assume the state flow value has the following existing properties:
// title: "Default title"
// doneButtonEnabled: false

// Label: Launch A
viewModelScope*.*launch*(Dispatchers.IO) **{** val currentValueA = _viewState.value
    val newValueA = currentValueA.copy(doneButtonEnabled = true)
    _viewState.value = newValueA
**}** *// Label: Launch B
viewModelScope*.*launch*(Dispatchers.Default) **{** val currentValueB = _viewState.value
    val newValueB = currentValueB.copy(title = "New Title")
    _viewState.value = newValueB
**}**
```

让我们浏览一些场景:

## 场景 1:串行执行

这个场景非常简单。定时工作使得`Launch A`在`Launch B`之前运行。`Launch A`λ将状态流量值更新为:

```
title: Default Title
doneButtonEnabled: true
```

然后，当`Launch B` lambda 运行时观察该值，并且它依次将状态流值更新为

```
title: New Title
doneButtonEnabled: true
```

这里没有真正的问题。价值观是一致的，但真的只是时间上的运气。

## 场景 2:并发执行

这是事情变得有趣的场景。在这种情况下，两个块同时运行。两者都将初始状态流值视为

```
// Initial state flow value
title: Default Title
doneButtonEnabled: false
```

`Launch A`中的`copy`函数复制初始数据类，覆盖`doneButtonEnabled`属性，然后块发出以下状态流值:

```
// Data class produced by the Launch A lambda
title: Default Title
doneButtonEnabled: true
```

大约在同一时间，`copy`函数`Launch B`复制它在`Launch A`发出值之前读取的初始数据类，覆盖`title`属性，然后块发出以下状态流值:

```
// Data class produced by the Launch B lambda
title: New Title
doneButtonEnabled: false
```

注意到问题了吗？两个块中只有一个会“赢”——无论哪个最后发射。两个块都读取当前 UI 状态，并且两个块都试图只更新 UI 状态的一部分，保留数据的重置。但这不会发生。

不跳线程会出现这种情况吗？答案是*是*，如果复制的结果在被发送到流之前被保存任意长的时间。然而，最常见的情况是，当有多个并发的状态流访问/读取来构建复制的数据类，然后在状态流上有多个并发的数据类发出。

# 一些解决方案

那么我们如何解决这个问题呢？

## 互斥（体）…

想到的第一个可能的解决方案是用某种同步(如`mutex`)来包围状态流的所有更新，这样两个并发的状态流值读取和更新就不可能发生。实施此解决方案后，示例现在变成了:

```
val mutex = *Mutex*()*// Assume the state flow value has the following existing properties:
// title: "Default title"
// doneButtonEnabled: false

// Label: Launch A
viewModelScope*.*launch*(Dispatchers.IO) **{** mutex.withLock **{** _viewState.value = _viewState.value.copy(doneButtonEnabled = true)
    **}
}** *// Label: Launch B
viewModelScope*.*launch*(Dispatchers.Default) **{** mutex.withLock **{** _viewState.value = _viewState.value.copy(title = "New title")
    **}
}**
```

这是一个不错的解决方案。开发人员必须记住维护互斥或其他同步方法以及围绕它的规则。(例如，互斥锁是不可重入的，所以要小心！)

## 比较和设置

另一个，在我看来，更优雅的解决方案是使用从 [Kotlin Coroutines 版本 1.5.1](https://github.com/Kotlin/kotlinx.coroutines/releases/tag/1.5.1) 开始`MutableStateFlow`可用的新扩展函数，所有这些都使用了`[compareAndSet](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.flow/-mutable-state-flow/compare-and-set.html)`函数。

`MutableStateFlow`的`compareAndSet`功能并没有被很多开发者真正注意到。从表面上看，在设置一个值时，它的用处并不明显。新的扩展功能对此有所帮助。

新功能是`[*getAndUpdate*](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.flow/get-and-update.html)`、`[update](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.flow/update.html)`和`[updateAndGet](https://kotlin.github.io/kotlinx.coroutines/kotlinx-coroutines-core/kotlinx.coroutines.flow/update-and-get.html)`

所有三个扩展函数都有一个函数参数，将调用该参数来生成要在状态流上发出的下一个值。实施此解决方案后，我们的示例变成:

```
*// Assume the state flow value has the following existing properties:
// title: "Default title"
// doneButtonEnabled: false

// Label: Launch A
viewModelScope*.*launch*(Dispatchers.IO) **{** _viewState.update { it.copy(doneButtonEnabled = true) } **}** *// Label: Launch B
viewModelScope*.*launch*(Dispatchers.Default) **{** _viewState.update { it.copy(title = "New title") } **}**
```

现在干净多了。但是，如何保证并发访问的安全性呢？查看`update`函数的源代码，我们有以下内容:

```
public inline fun <T> MutableStateFlow<T>.update(function: (T) -> T) {
    while (true) {
        val prevValue = value
        val nextValue = function(prevValue)
        if (compareAndSet(prevValue, nextValue)) {
            return
        }
    }
}
```

我们可以看到，在尝试比较和设置之前，一个高阶函数作为参数传入并应用于先前的状态流值，以创建新的*值。然后在设置一个新值之前，检查先前的值是否已经改变，比如说被另一个线程改变。如果这个检查失败了，那么`update`函数只是忙循环再试一次，直到该值可以被实际设置。如果在当前线程执行时，另一个线程改变了先前的值，那么用于创建新的`StateFlow`值的高阶函数被重复。*

这样，数据类的`copy`函数以新数据结束，然后可以安全地设置`StateFlow`值，同时仍然保留未修改的属性。

更新 StateFlow 的值可能会带来一些隐藏的陷阱，尤其是当有并发操作正在进行时。使用一个`MutableStateFlow update`扩展函数可以减轻这些问题，而不需要使用冗长且可能阻塞线程的互斥锁。

然而，互斥可能有它的位置，这取决于你的用例，所以不要完全忽略它。