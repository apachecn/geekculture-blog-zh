# 简化 Android 中微调器的使用

> 原文：<https://medium.com/geekculture/simplifying-using-spinners-in-android-ad14f8f1213d?source=collection_archive---------5----------------------->

## 一个漂亮的小类，它将在你的 Android 应用程序中使用 spinners 来简化一堆东西(当然是在 Kotlin 中！).

![](img/ccefb0d596602fd67b40b56fa840d7b2.png)

Photo by [ian dooley](https://unsplash.com/@sadswim?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 Android 中创建用户体验时，pinners 是非常有用的小部件。当用户必须在不同的选项中进行选择时，它们向用户清楚地指示了哪些选项是可用的，并且一旦用户做出选择，它们就通过折叠来节省小屏幕的空间。然而，不幸的是，将使用 spinner 的代码组合在一起涉及到处理一些样板代码:这些代码并不描述您试图实现的功能，而只是为了处理在 Android 中使用 spinner 的一些复杂性，以及必须在许多地方重复的代码。

不过，幸运的是，编程的一个好处是，你总能找到摆脱重复性家务的方法。在本文中，我展示了一小段代码，当您只需要一个简单的微调器向用户显示一些选项的文本并返回与用户选择的选项相对应的字符串值时，您可以在许多地方重用这段代码。这一切都被封装在一个名为`BindableSpinnerAdapter`的类中(我将第一个承认想出吸引人的名字不是我的强项)，你可以在下面看到:

请注意，这个类被认为是 MVVM(模型-视图-视图模型)架构:如果你的应用程序还没有出现，也许这会给你一个动力去实现它！因此，这个类假设将会有一个`ViewModel`对象，并且这个`ViewModel`对象可以通过数据绑定在你的屏幕布局中使用。考虑到这些假设，`BindableSpinnerAdapter`类使开发人员的工作变得非常简单:您不需要创建一个适配器，事实上您甚至不需要对`Activity`类中的旋转器做任何事情。只需将上面的代码文件放入您的项目中(或者，如果您愿意，您可以将上面的代码包含在一个漂亮的 Android 实用程序小库中)，只需在您的`Activity`布局中添加这一点小小的更改，一切都会自动为您完成:

```
<Spinner
 […]
 spinnerItems=”@{viewModel.items}”
 selectedSpinnerItem=”@={viewModel.selectedItem}” />
```

**注意:**`selectedSpinnerItem``InverseBindingAdapter`中的`“@={…}”`很重要。这向 XML 处理器表明这是一个**逆** `BindingAdapter`。没有这一点，这个代码不能工作。

在你的`ViewModel`中，将`items`和`selectedItem`属性暴露为`LiveData`，就有了。然后，要填充微调器，只需向`items`发送一个对应于`BindableSpinnerAdapter.SpinnerItem`对象列表的值就可以了。`SpinnerItem`对象包含两个属性:一个要在微调器中显示的文本，一个值，对应于与微调器选项对应的任何字符串值(例如，需要通过网络发送到 API 的特定字符串代码)。

为了在用户选择微调器中的一个选项时得到，你可以只依赖于`ViewModel`的`selectedItem`属性。请注意，每次用户选择一个选项时，都会调用`LiveData`属性和您的代码。但是，请记住您不能在`ViewModel`中使用`lifecycleOwner`对象:要做的是使用`LiveData.map()`调用来转换`selectedItem` `LiveData`属性。例如，将`selectedItem`转换为`spinnerOptionChanged` `LiveData`属性，并且不要忘记在您的`Activity`中观察`spinnerOptionChanged`(`LiveData`对象如果在某个地方没有`lifecycleOwner`就不起作用)。

这样，向应用程序的用户界面添加微调器就变得非常容易，并且您可以去掉许多样板代码。当然，上面的类并不完美:如果我有更多的时间，我想我可以对它做更多的改进(也许使用一些泛型？…)但我认为它确实能帮你不少忙。尽管如此，请随意提出修改和/或改进的建议，或者将此代码更改为最适合您的代码。这就是开发人员共享代码的美妙之处:一起工作可以让我们所有人的工作变得简单一些。编码快乐！