# 在 Jetpack 合成列表中集成谷歌地图

> 原文：<https://medium.com/geekculture/integrate-google-maps-in-jetpack-compose-list-681c97ec788f?source=collection_archive---------6----------------------->

![](img/ef7bbeedc43f6f9821eb9b180a36d3ee.png)

在这篇文章中，我将向你展示如何在 jetpack 编写列表中添加谷歌地图。我推荐你阅读我以前的一篇关于在 jetpack compose android 中集成谷歌地图的文章。

[https://daanidev . medium . com/Google-maps-in-jetpack-compose-Android-AE 7 B1 ad 84 e 9](https://daanidev.medium.com/google-maps-in-jetpack-compose-android-ae7b1ad84e9)

因此，现在您只需要创建一个带有 jetpack compose empty 活动的项目，并为 google maps 添加这些库

```
implementation("com.google.android.libraries.maps:maps:3.1.0-beta")
implementation("com.google.maps.android:maps-v3-ktx:2.2.0")
implementation("androidx.fragment:fragment:1.3.2")
```

现在，您需要在 res 包中创建布局包，并使用以下代码在其中创建新的资源文件

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <androidx.fragment.app.FragmentContainerView
        xmlns:android="http://schemas.android.com/apk/res/android"
        android:id="@+id/map"
        android:name="com.google.android.gms.maps.SupportMapFragment"
        android:layout_width="match_parent"
        android:layout_height="match_parent"/>

</androidx.constraintlayout.widget.ConstraintLayout>
```

现在创建 MapUtils 文件，并在该文件中添加以下代码

然后创建数据类，它将负责在你的列表中添加动态数据

```
data class MapModel (val pickUp:LatLng,val destination:LatLng,val date:String,val time:String
,val pickUpTitle:String,val destinationTitle:String)
```

创建数据类后，创建您的存储库类，并在其中添加以下函数。

现在，在 mainactivity.kt 的 oncreate 方法内的主题块中添加以下代码

现在，最后在 mainactivity.kt 中添加下面的可组合函数

不要忘记在你的清单文件中添加下面的标签。

```
<uses-permission android:name="android.permission.INTERNET"/><meta-data android:name="com.google.android.geo.API_KEY"
    android:value="api_key"/>
```

![](img/734f3f74289bc5467d11e17320da2d53.png)

Output Result

如果您遇到任何问题，请查看 JetPack 撰写列表中的谷歌地图

[https://github . com/DaaniDev/googlemapinsidelist _ jetpackcompose . git](https://github.com/DaaniDev/googlemapinsidelist_jetpackcompose.git)