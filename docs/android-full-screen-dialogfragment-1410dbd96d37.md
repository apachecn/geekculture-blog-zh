# Android:全屏对话片段

> 原文：<https://medium.com/geekculture/android-full-screen-dialogfragment-1410dbd96d37?source=collection_archive---------1----------------------->

![](img/16e75a36afa4661c87f11b8c12e1d795.png)

Photo by [Volodymyr Hryshchenko](https://unsplash.com/@lunarts?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

对话框一直是在同一个屏幕上向用户显示快速细节的直观方式，有时我们需要对话框占据整个屏幕。有多种方法可以显示全屏对话框，但有时会出现与所有操作系统版本兼容的问题，或者有时我们需要编写大量代码来实现这一点。

我将向您展示最简单、最快的获取方法:

在 styles.xml 文件中添加以下样式:

```
<style name="DialogTheme" parent="AppTheme">
        <item name="android:windowNoTitle">true</item>
        <item name="android:windowFullscreen">false</item>
        <item name="android:windowIsFloating">false</item>
</style>
```

在扩展 DialogFragment 的类中重写以下方法:

```
override fun getTheme(): Int {
    return R.style.*DialogTheme* }
```

就是这样。

你有一个全屏的对话片段。