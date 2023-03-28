# 在 ServiceNow 中检测记录是从桌面还是移动设备创建的

> 原文：<https://medium.com/geekculture/detecting-if-a-record-was-created-from-a-desktop-or-mobile-device-4a3f95ac702d?source=collection_archive---------29----------------------->

## 使用 isMobile()方法检查记录是否是从移动设备创建的。

![](img/98b6b2a97a48c49e7a39d51b8d906e75.png)

Photo by [Firmbee.com](https://unsplash.com/@firmbee?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/mobile-phone?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

我正在处理一个需求，我需要知道记录是从移动设备还是桌面设备创建的，因此，我想尝试一下 JavaScript navigator 方法，以检测记录是从移动设备还是桌面设备创建的，并发现 ServiceNow 有一个来自 GlideSystem 类的方法来完成这个任务。GlideSystem 提供了几种有用的方法来获取关于系统、当前登录系统的用户以及日期和时间函数的信息。尽管这个脚本只适用于新创建的记录，但是了解这个方法以便将来使用还是很有好处的。

所以，现在，isMobile()方法。

首先，我们在*事件*表中创建一个新字段，称为“u_source”选择列表，有两个选项；“移动”和“桌面”

![](img/aae64f52fe84ff8c7b9c1dbcf8cfe0b5.png)

然后，我们为每次插入运行的*事件*表创建一个 before 业务规则。

我们用`gs.isMobile()`检查记录是否是从移动设备上创建的。如果它返回 true，那么我们使用`current`对象；**当前对象**从 GlideRecord 类实例化而来，可以访问记录的所有字段和所有 GlideRecord 方法。因为我们刚刚创建了一个新的字段，它应该可以使用`current.u_source;`来访问，在这种情况下，我们希望在条件返回 true 的实例中，为字段 *u_source，*赋值*e“Mobile”。*

```
if (gs.isMobile())
     current.u_source = 'Mobile';
 else
     current.u_source = 'Desktop';
```

业务规则的完整主体:

```
(function executeRule(current, previous /*null when async*/ ) {
 if (gs.isMobile())
     current.u_source = 'Mobile';
 else
     current.u_source = 'Desktop';
})(current, previous);
```

通过创建新的事件记录从桌面测试输出，将*源*字段设置为“桌面”，当从移动设备创建事件时，将“源”字段设置为“移动”。