# Android:使用 styles / themes.xml 的闪屏

> 原文：<https://medium.com/geekculture/android-splash-screen-using-styles-themes-xml-6997f2f8c09e?source=collection_archive---------33----------------------->

![](img/0ac83a0a484b27708aed40c5b8ae3b16.png)

Photo by [Marcus Ganahl](https://unsplash.com/@marcus_ganahl?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在我之前的一篇[文章](https://anubhav-arora.medium.com/android-quick-and-simple-splash-screen-ba1d82ccb22d)中，我们谈到了用传统的方式创建一个闪屏。在本文中，我将讨论一种通过使用 styles/themes.xml 实现闪屏的方法，这将消除创建另一个活动来显示闪屏的需要。

因此，首先我们需要在 styles/themes.xml 文件中为闪屏创建一个样式。

```
<style name="**SplashScreenTheme**" parent="**Theme.MaterialComponents.DayNight.NoActionBar**">
    <item name="**android:windowBackground**">@drawable/ic_splash_screen</item>
</style>
```

我创建了一个名为 SplashScreenTheme 的样式，其父样式没有动作栏。我添加了一个属性为 **windowBackground，**的项目，这是我们需要放置闪屏样式/ drawable 的地方。我正在使用我的 drawable 文件夹中的图像。

为了在我们的启动器活动之前显示闪屏，我们需要用我们刚刚创建的主题替换我们的应用程序主题。
转到清单文件，将应用程序主题设置为 **SplashScreenTheme，**

```
<application
    ...
    **android:theme="@style/SplashScreenTheme"**>

    ....

</application>
```

现在我们的应用程序风格被设置为 **SplashScreenTheme** 。

接下来，在闪屏显示完成后，给我们的应用程序我们的旧主题，我们需要设置我们之前在启动器活动中使用的应用程序主题 **onCreate(..)**法。

```
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    **setTheme(R.style.*YOUR_THEME*)**

    ....}
```

> 在我们的启动器活动 onCreate(..)被称为我们显示闪屏主题，在它的 onCreate(..)叫做我们切换到我们的 app 主题。

如果我们现在运行我们的应用程序，闪屏将会出现。
如果你注意到我们可以在闪屏上看到状态栏和导航按钮，我们可以隐藏它们以给用户更好的体验。同样，我们需要在我们创建的主题中再设置两个属性，

```
<style name="SplashScreenTheme" parent="Theme.MaterialComponents.DayNight.NoActionBar">
    <item name="android:windowBackground">@drawable/ic_splash_screen</item>
    **<item name="android:windowTranslucentStatus">true</item>
    <item name="android:windowTranslucentNavigation">true</item>**
</style>
```

上述属性是不言自明的，它们使状态栏和导航栏半透明。

就是这样，伙计们，您已经启动并运行了闪屏，无需创建另一个活动。