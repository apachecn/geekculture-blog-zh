# 使用带有 SwiftUI 颜色和一些 NSKeyedArchiver 魔法的@AppStorage

> 原文：<https://medium.com/geekculture/using-appstorage-with-swiftui-colors-and-some-nskeyedarchiver-magic-a38038383c5e?source=collection_archive---------2----------------------->

## 符合`RawRepresentable using a NSKeyedArchiver`

![](img/0dcb165729d0a4b94abd0539b291a3a7.png)

Photo by [Yousef Espanioly](https://unsplash.com/@yespanioly?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

iOS 14 为 SwiftUI 带来了一些令人兴奋的新功能，我最喜欢的功能之一是`@AppStorage`包装器。这个新的属性包装器使得以`UserDefaults`方式存储值变得更加容易。在 iOS 14 之前，这必须通过使用 getter 和 setter 方法来完成。

> [**@AppStorage 文档**](https://developer.apple.com/documentation/swiftui/appstorage)

@AppStorage 原生支持`Bool`、`Int`、`String`、`URL`、&、`Data`。但是如果想要显示更复杂的数据类型呢？比如一个 SwiftUI `Color`。我在开发自己的应用程序时也遇到了这个问题。我想在`UserDefaults`中为某些内容存储一个默认的色调，使用`ColorPicker`选择(这也是 iOS 14 的新功能)。

为了支持一个非原语或自定义类，您必须扩展类型以符合`RawRepresentable`。该协议指定了用于从原始类型初始化值的自定义初始值设定项，以及用于将对象实例转换为原始类型的公共变量。为了符合这个协议，不幸的是，我们不能使用一些更高级的类型，比如`Data`。我们被限制使用一个`String`或`Int`作为我们的原始类型。

没关系，使用整数作为类型来存储颜色是很常见的，以前也有人这样做过。不幸的是，这并不像初看起来那么容易。我的第一个想法是使用一个 64 位整数以字节形式存储 4 个颜色分量，格式如下`RRRRRRRR GGGGGGGG BBBBBBBB AAAAAAAA…`。为了保存到`UserDefaults`，我将从 SwiftUI `Color`中提取颜色成分(通过转换到`CIColor`)，然后将这些成分转移到整数的正确部分。

> 如果你不知道什么是比特移位，这里有一个快速纲要:[https://en.wikipedia.org/wiki/Logical_shift](https://en.wikipedia.org/wiki/Logical_shift)

一个 64 位整数可以本地存储在`UserDefaults`中，所以下一步是简单地反转这个过程，然后在需要时从提取的组件中重建一个新的 SwiftUI `Color` 对象。Tada！一个非常简单的方法来转换和存储一个`Color`作为一个原始的(和回来)。这里有一个简单的实现示例。

## **转换为整数**

```
let red = Int(coreImageColor.red * 255 + 0.5)
let green = Int(coreImageColor.green * 255 + 0.5)
let blue = Int(coreImageColor.blue * 255 + 0.5)
return (red << 16) | (green << 8) | blue
```

## **变回一种颜色**

```
let red = Double((rawValue & 0xFF0000) >> 16) / 0xFF
let green = Double((rawValue & 0x00FF00) >> 8) / 0xFF
let blue =  Double(rawValue & 0x0000FF) / 0xFF
self = Color(red: red, green: green, blue: blue)
```

[原要旨](https://gist.github.com/vibrazy/b105e3138105f604ab1ee4cfcdb67075)

不幸的是，事情没那么简单。提取到组件并返回，通常会导致一些浮点错误。0 值通常表示为非常小的数字，这可能会导致一些非常奇怪的意外行为。我尝试锁定值的范围(0-255 ),但是我仍然得到一些非常奇怪的 UI 行为。

## **我运行 app 时发生了什么？**

不完全是一个伟大的用户体验，对不对？更改一个滑块会更改其他滑块。甚至改变一个滑块都不可靠。关闭并终止应用程序任务，数据甚至得不到可靠的维护。值会稍微高于或低于选取器上选择的值。原始类型和颜色之间的值似乎可以正确转换，那么是什么呢？

我的猜测是，移动这些位来执行 raw 和 color 转换，以及转换到一个`CIColor`来提取 raw color 值会导致内存崩溃。

我决定尝试使用好的 ol' `NSKeyedArchiver`类来归档一个实际的对象。这通常与`UserDefaults`一起使用，它可以省去我们直接与红色、绿色、蓝色和不透明度值交互的痛苦。我们不能原生编码一个`Color`，因为它不是一个`NSObject`，但幸运的是一个`UIColor`是。

`UIColor`现在也有了一个初始化器，它将 SwiftUI `Color`作为参数。这使得 SwiftUI `Color`和`UIColor`之间的转换变得轻而易举。我们简单地将一个`Color`转换成一个`UIColor`，然后使用`NSKeyedArchiver`将对象存档到`Data`，最后，我们可以 [Base64](https://en.wikipedia.org/wiki/Base64) 这个`Data`将它转换成 raw `String`。

我们可以使用`NSKeyedUnarchiver`反向执行相同的过程，将`String`恢复为 SwiftUI `Color`。

**这是我想出的代码**

使用这个扩展，你可以很容易地设置一个 SwiftUI `ColorPicker`，保存到`UserDefaults`并在两次启动之间维护。这里有一个快速的示例视图。

这就是最终的结果！🎉🎉🎉

## 有什么区别？

使用`NSKeyedArchiver`的好处是没有任何色彩成分的干扰。我们做的任何事情(在某种程度上)都无法打破。全靠苹果的`NSKeyedArchiver` & `NSKeyedUnarchiver`。我们不需要做任何位移或逐位运算，因为所有的转换都是为我们处理的。缺点是存储在设备上时的大小大约是`400 bytes`而不是`8 bytes`。

为什么会有巨大的差异？请记住，我们并不像最初的位移位方法那样只存储颜色值。我们正在存储一个`NSObject`的实际实例。`NSKeyedArchiver`将对所有东西进行编码，包括编码对象的键及其任何底层类型。这将占用相对较大的内存量，但这意味着原始类型之间的转换是直接的，而不是通过一个复杂且可能更容易出错的转换过程。

你可以在下面看到`NSKeyedArchiver`的输出被转换成了 ASCII 码。有一堆特殊字符，但你可以分辨出一些零碎的字符，如`ColorComponentCount` 和`NSColorSpace`。(意料之中)`Red`、`Green`和`Blue`这些词也会出现。如果每个字符占用一个字节(假设 UTF-8 编码)，那么我们很容易就有几百个字节。

仔细看看，你还会看到这里有一些实际值，如`0.599`和`0.255`。如果不使用一些工具深入研究数据，很难看出这些颜色与存储的颜色到底是如何对应的，但是与试图将所有东西都分解成一个整数相比，它可以让您很好地了解事物是如何被编码的。

很可能有一种方法可以将 SwiftUI `Color`存储在一个整数或字符串中，而不会导致奇怪的 UI 错误。如果有人有一个方法运行良好，不会引起奇怪的 UI 行为；请分享。但在那之前，我很乐意牺牲一点内存来获得更好的性能和功能。

我希望这一切教会了你一些东西，我确实学到了一点。🧠

如果你喜欢这篇文章，请在 [Twitter **上关注我们🐦**。](https://twitter.com/iamzanecarter)