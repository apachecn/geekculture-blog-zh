# Android:圆形图像视图

> 原文：<https://medium.com/geekculture/android-circular-imageview-4147579d1a44?source=collection_archive---------24----------------------->

![](img/5c33e0f093bc6fa68d370cfab62f2baf.png)

Photo by [Erlend Ekseth](https://unsplash.com/@er1end?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

许多 android 应用程序使用圆形 ImageView 来显示个人资料图片、状态反馈等，但使用 android [ImageView](https://developer.android.com/reference/android/widget/ImageView) 小部件来实现这一点并不简单。有一个非常流行的第三方[库](https://github.com/hdodenhof/CircleImageView)帮助实现同样的功能，但是在这个片段中，我们不会使用任何第三方库，**它是** **快速而简单。**

我将使用一个 [CardView](https://developer.android.com/reference/androidx/cardview/widget/CardView) 来实现同样的功能。下面是相同的代码片段。

```
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout 
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content">

    <androidx.cardview.widget.CardView
        android:id="@+id/cvStories"
        android:layout_width="60dp"
        android:layout_height="60dp"
        app:cardCornerRadius="30dp"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent">

        <ImageView
            android:id="@+id/ivStory"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:scaleType="centerCrop"
            android:background="@drawable/placeholder_image"
            android:contentDescription="@string/story_image"
            tools:background="@android:color/holo_orange_light" />

    </androidx.cardview.widget.CardView>

</androidx.constraintlayout.widget.ConstraintLayout>
```

在 CardView 中，我添加了一个带有可绘制源代码的 ImageView。

这里要注意的要点是:

**layout_width** 和 **layout_height** 的值必须相等，以使 ImageView 呈圆形， **app:cardCornerRadius** 的值必须至少是 **layout_width / layout_height，**的一半，这样就可以了。