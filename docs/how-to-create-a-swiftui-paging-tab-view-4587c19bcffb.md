# 如何创建 SwiftUI 分页选项卡视图

> 原文：<https://medium.com/geekculture/how-to-create-a-swiftui-paging-tab-view-4587c19bcffb?source=collection_archive---------5----------------------->

## SwiftUI 教程

## Swift 分页集合视图的 SwiftUI 版本。

![](img/210599d077a2d6e08fe0ce3939852a8a.png)

My daily process

我在做一个纯粹的 SwiftUI 项目，它有一个搜索栏，一个地图，然后是一个结果部分。结果部分需要是用户可以刷卡的卡片集合，当用户停止刷卡时，焦点会捕捉到中心项目(如照片)。然后，中心项目将更新地图中的大头针。

在 Swift 中，这可以通过使用“`section.orthogonalScrollingBehavior = .groupPagingCentered`”的集合视图轻松完成

但是，当我在 SwiftUI 中搜索这样做的方法时，我没有找到一个简单的答案。

我尝试在 ScrollView 中使用 LazyHGrid，但是它不能捕捉到中心项目，也不能提取中心项目的索引来更新地图。

所以我创建了一个自定义的视图修改器，并在 HStack 上使用。这解决了对齐中心的问题，但它并不总是有效，而且很难让卡片看起来正确。再加上还是没有解决索引问题。

所以我咬紧牙关，将集合视图转换为 UIViewRepresentable(这里有一组很好的指令[和](https://defagos.github.io/swiftui_collection_intro/))，这让它很好地捕捉到项目，但我仍然不能可靠地获得居中的索引。

昨晚，我突然灵光一现。我正在摆弄标签视图，我看到了这个[帖子](https://swiftwithmajid.com/2020/09/16/tabs-and-pages-in-swiftui/)展示了如何用分页风格制作标签视图。

我想知道 TabView 是否只用于整页项目，或者它是否可以在另一个视图中工作。所以我试了一下，瞧！很好用！

# 教程项目

我创建了一个示例项目，所以你也可以测试它。本教程假设您对 SwiftUI 有些陌生，但至少对 Swift 有基本的了解。只是想看看选项卡视图的实现？跳到[底部](#a46b)。

Search App with paging Tab View

## 模型和数据

首先，在 Xcode 中创建新的 iOS App 项目。确保为界面选择“SwiftUI”，为生命周期选择“SwiftUI App”。

在名为“Models.swift”的内容视图下创建一个新的 Swift 文件。这是我们将创建地点和假期模型的地方。

现在让我们制作一些模拟数据进行搜索。您可以将其添加到模型文件中，或者创建一个“MockData.swift”文件。我为图像添加了随机的系统图片。

## 初始设置

现在我们已经建立了数据，让我们开始创建视图。

在 ContentView.swift 文件下，新建 4 个 SwiftUIViews:“searchbarview . swift”、“SearchMapView.swift”、“SearchResultsView.swift”和“ResultCardView.swift”。现在，将每个文件主体中的默认文本更改为文件名，这样您就可以在测试过程中看到视图。

打开 ContentView，添加两个状态属性，一个用于结果，一个用于当前选择的结果索引。然后在主体中，创建 SearchBarView、SearchMapView 和 SearchResultsView 的 VStack。

它应该是这样的:

## 搜索栏

搜索栏视图有几个组成部分:标题、文本字段和搜索功能。

为我们希望用于搜索的输入文本创建一个 State 属性，并为结果创建一个绑定。在正文中，添加一个带有文本标题和文本字段的 VStack。文本字段将链接到 State 属性中的文本。然后，您可以添加一个按钮来开始搜索，或者让文本字段搜索“onCommit”。我觉得搜索 onCommit 更直观。

在我们创建搜索功能之前，让我们在文件底部添加一个小助手。这个助手非常适合查找数组中满足特定条件的所有值。我在这里找到了这个帮手[。](https://learnappmaking.com/find-item-in-array-swift/#finding-all-items-in-an-array-with-allwhere)

```
**extension** Array **where** Element: Equatable {
 **func** all(where predicate: (Element) -> Bool) -> [Element]  {
 **return** **self**.compactMap { predicate($0) ? $0 : **nil** }
  }
}
```

现在我们可以使用 all(where:)方法来动态过滤我们的数据。我选择让它在 Vacation.name 或 Place.name 中查找搜索词

## 地图

[本节于 2011 年 4 月 8 日更新，以正确更新地区。]

为了让地图动态更新，我们需要创建一个 ObservableObject 模型和一个 ObservedObject 属性。使用可观察对象可以防止视图在视图更新期间修改状态，这可能会产生意想不到的结果。首先，让我们创建区域模型:

然后将 ObservedObject 作为属性添加到地图中。我为我的国家给它一个缺省值，但是你能在你喜欢的任何地方设置你的。

我们还需要结果和索引的绑定。

然后，每当结果或当前选择发生变化时，我们将需要一个函数来重新居中和缩放地图。

当我们创建地图时，我们希望注释根据当前选择的结果进行更新。但是我们不能指望结果总是有一个值。在用户搜索之前，或者如果搜索是空的，地图不应该有任何注释。annotationItems 将接受空数组，但不接受可选的/nil 项。

## 成绩卡

现在我们需要制作显示每个结果的卡片。对于本教程，我决定在假期中添加随机的系统图片，这样卡片会更有视觉吸引力。你可以随意调整这张卡片的设计。我很想看看人们想出的一些创造性的东西。

## 搜索结果视图

现在项目已经设置好了，我们终于可以在搜索结果视图中实现 TabView 了。我们需要绑定结果和选择属性。实际的选项卡视图非常容易实现。

让我们从构建视图的主要部分开始。

构建选项卡视图类似于在 SwiftUI 中创建列表，因为您创建它，并使用 ForEach 迭代数组。神奇之处在于第一行，在这里，TabView 被绑定到选择。它将根据您当前查看的结果自动更新选择。

```
TabView(selection: $selection) {}
```

在这种情况下，我们将稍微不同地实现 ForEach，因为我们希望将当前显示的卡片的索引传递给 map，因此它将更新 pin。使用 results.enumerated()来做到这一点很有诱惑力，但我警告不要这样做。特别是当数组改变时(你做了一个新的搜索)，有可能得到一个“索引越界”的错误。所以我们将压缩数组，就像 [this](https://stackoverflow.com/questions/59295206/how-do-you-use-enumerated-with-foreach-in-swiftui) :

```
ForEach(Array(zip(results.indices, results)), id: \.0) { index, result **in
**  ResultCardView(vacation: results[index]).tag(index)
}
```

标签非常重要，因为这是 TabView 知道将选择更新到哪个数字的方法。

现在给 TabView 一些样式

```
.tabViewStyle(PageTabViewStyle(indexDisplayMode: .always))
```

完成的 TabView 应该如下所示:

```
TabView(selection: $selection)  {
  ForEach(Array(zip(results.indices, results)), id: \.0) { index, result **in** ResultCardView(vacation: results[index]).tag(index)
  }
} //: TabView
.tabViewStyle(PageTabViewStyle(indexDisplayMode: .always))
```

现在项目完成了！通过运行来测试它，并告诉我你的想法。在此找到已完成的项目[。](https://github.com/CrystalKnightCodes/SearchResults)