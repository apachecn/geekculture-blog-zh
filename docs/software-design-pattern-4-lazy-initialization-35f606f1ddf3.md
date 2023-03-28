# 软件设计模式#4:惰性初始化

> 原文：<https://medium.com/geekculture/software-design-pattern-4-lazy-initialization-35f606f1ddf3?source=collection_archive---------23----------------------->

设计模式是可重用的模板，帮助我们使用最佳实践解决软件设计问题。通过这种方式，它们帮助我们使用更易于维护、理解、重用和测试的代码来构建应用程序。

# 逃离速度实验室

你可以在我们的网站上找到我们所有的文章、课程和教程:
[https://www . ev labs . io](https://www.evlabs.io/)

![](img/3528ae510f540e7781d1eff2ef2b7bfa.png)

# 这个图案是干什么用的？

> 这是一种策略，将对象的创建、值的计算或任何其他计算量大的过程延迟到第一次使用它的时候。

让我们回到我们在本系列的前几篇文章中看到的社交网络的例子。我们想添加一个功能，显示我们的联系人和这些联系人的联系(喜欢，评论，视频，照片等)的活动。收集这些信息是一个相当昂贵的过程，在这个过程中，我们必须遍历我们的联系人列表，并保存他们每个人的活动，以及他们所有联系人的活动。因此，该功能消耗大量资源，包括内存和计算时间。

一种选择是在启动应用程序时建立这个活动列表，但这将增加用户在加载屏幕上花费的时间。这是一个非常糟糕的想法，因为需要几秒钟以上加载的应用程序通常会很快被卸载。此外，用户在会话期间可能不会使用此功能。

**解决方案**:在用户需要的时候创建活动列表。

# 它是如何工作的？

我们第一次需要昂贵资源的时候就是我们创造它的时候。在我们的例子中，该资源是联系人活动列表，我们将把它保存为用户配置文件中的一个属性。因此，当创建概要文件时，属性“contact_list”将被初始化为 *None* (在其他语言中为 *null* )。

Profile 类中提供对该活动列表的访问的方法将包含一个 *if* 语句，在该语句中我们将检查该列表是否已经被创建。如果它已经被创建了，因为它以前被使用过，我们将简单地返回那个列表。万一这个列表不存在，因为它是第一次被使用，我们将创建它并返回它。

我们可以将列表创建逻辑封装在一个辅助的私有方法中，如下例所示:

```
**class** **Profile**:     **def** **__init__**(self):
        self.contact_activity = None
        ... **def** **get_contact_activity**(self):
        **if** **not** self.contact_activity:
            self.contact_activity = self._build_contact_activity()
        **return** self.contact_activity    

    **def** **_build_contact_activity**(self):
        ...
```

在应用程序中，当我们创建个人资料时，不会生成列表。如果用户不需要，则不会随时创建。另一方面，如果在某个时候需要它，将创建列表:

```
**if** __name__ == "__main__":
    profile = Profile()
    ...        # When we need the activity list for the first time.
    contact_activity = profile.get_contact_activity()
```

# 利益

*   通过将复杂的计算分散到不同的时间来加快启动时间。
*   除非必要，否则避免执行不经常执行的操作。