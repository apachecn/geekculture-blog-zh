# 在 SwiftUI 中使用 MapKit 进行搜索

> 原文：<https://medium.com/geekculture/search-using-mapkit-in-swiftui-636426836ab?source=collection_archive---------3----------------------->

![](img/f157338c74fe601fe942ed7a2775eb13.png)

Photo by [Z](https://unsplash.com/@dead____artist?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

搜索地点是我们在应用程序中可能需要的一个有趣特性。让我们用 SwiftUI 开发这个特性。

首先，让我们用一个确认可观察对象的类创建一个视图模型。这个类有一个发布搜索结果的 searchResults 变量。LocationManager 向“MKLocalSearchCompleterDelegate”确认，后者通过“completerDidUpdateResults”提供搜索结果。

getDistance 方法用于计算当前位置和搜索结果之间的距离。

我们可以进一步选择结果类型，并选择是否包括兴趣点。

```
self.searchCompleter.resultTypes = [.address]self.searchCompleter.pointOfInterestFilter = .includingAllself.publisher = $search.receive(on: RunLoop.main).sink(receiveValue: { [weak self] (str) inself?.searchCompleter.queryFragment = str})
```

现在，让我们创建一个显示上述结果的简单列表，并计算结果与当前位置的距离。

上面视图中的 reverseUpdate()需要一个专用的 [post](/@maheshsai252/working-with-details-of-location-in-swiftui-faf7b139270b) ，在其中我们将从坐标中获取位置的细节，反之亦然。

感谢阅读:)