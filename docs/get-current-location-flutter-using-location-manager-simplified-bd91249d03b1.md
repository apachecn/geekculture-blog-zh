# 使用位置管理器|简化版获取当前位置抖动

> 原文：<https://medium.com/geekculture/get-current-location-flutter-using-location-manager-simplified-bd91249d03b1?source=collection_archive---------14----------------------->

![](img/15bda3d1d914eac9467328137e87f7fd.png)

Photo by [Element5 Digital](https://unsplash.com/@element5digital?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 嗨，

这是我，我们将看看如何从设备 Android，Ios 和 Web 上获取位置。

通过使用下面的颤振依赖性，这是非常简单的。

[](https://pub.dev/packages/location) [## 位置|颤振包

### 这个 Flutter 插件处理在 Android 和 iOS 上获取位置。它还提供回调，当位置是…

公共开发](https://pub.dev/packages/location) 

根据上述文档，我们可以获得设置权限，在设备中启用位置，并获得当前位置。

所以我在这里做了一些新的事情，创建一个管理器类，它仍然做同样的事情，但是以*最酷的方式以**高效的**方式。*

*![](img/8dd456dbbdcf3f5ab4689dfec4800a50.png)*

*Photo by [Josh Rakower](https://unsplash.com/@joshrako?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)*

*在开始之前，我来自 android kotlin 平台，所以我使用了我们在 android 开发中遵循的相同架构。*

*首先，在****pubspec . yml***中实现位置依赖**

```
**dependencies:
  location: ^3.0.0**
```

# **然后，**

**为位置创建一个状态。**

# **和**

**位置管理器应该是，**

**这就够了:)。**

# **最后，**

**无论你想去哪里，只要像下面这样打电话，**

**这里我们用最简单的方法从屏幕上获取当前位置。**

**欢迎任何建议。**

**感谢您的阅读；)**

****参考****

> **[https://www.youtube.com/](https://www.youtube.com/)**
> 
> **[https://www.google.com/](https://www.google.com/)**