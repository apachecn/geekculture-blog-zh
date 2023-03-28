# Android: SearchView 实现

> 原文：<https://medium.com/geekculture/android-searchview-implementation-1998f633681e?source=collection_archive---------1----------------------->

![](img/8b6d771cf35410d19532eac8f37e585a.png)

Photo by [Marten Newhall](https://unsplash.com/@laughayette?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**Search View** widget 为用户提供了一个搜索界面，让用户搜索一个查询，并向搜索提供者提交请求，获得一个查询建议或结果的列表。

在本文中，我将介绍 SearchView 小部件的实现。

让我们从第一个实现开始

按如下方式创建菜单文件:

```
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <item
        android:id="@+id/menu_search"
        android:icon="@drawable/ic_search"
        android:title="@string/search_menu"
        app:actionViewClass="androidx.appcompat.widget.SearchView"
        app:showAsAction="ifRoom" />

</menu>
```

这里要注意的主要事情是

```
app:actionViewClass="androidx.appcompat.widget.SearchView"
```

actionViewClass 定义了实现动作的小部件的类，在我们的例子中，它是 SearchView。

我们的活动或片段应该实现

```
SearchView.OnQueryTextListener
```

确保您实施了

*Android x . app compat . widget . search view*而不是
Android . widget . search view

该接口将要求我们覆盖以下方法:

```
override fun onQueryTextSubmit(query: String?): Boolean {
    *TODO*("Not yet implemented")
}

override fun onQueryTextChange(query: String?): Boolean {
    *TODO*("Not yet implemented")
}
```

*onQueryTextSubmit(查询:字符串？):*将在我们输入文本并在搜索视图中按回车键时被触发。

*onQueryTextChange(查询:字符串？):*将在我们开始在搜索视图中输入内容时被触发。

您可以根据自己的需求选择合适的方法。

然后在我们的活动或片段中覆盖 onCreateOptionsMenu 方法并添加以下逻辑。

```
override fun onCreateOptionsMenu(menu: Menu?): Boolean {
    *menuInflater*.inflate(R.menu.*main_activity_menu*, menu)
    val search = menu?.findItem(R.id.*menu_search*)
    val searchView = search?.*actionView* as SearchView
    searchView?.*apply {**isSubmitButtonEnabled* = true
      setOnQueryTextListener(this@AcitvityOrFragmentReference)
    }
}
```

*val 搜索=菜单？。findItem(r . id . menu _ search):*搜索菜单项

*val searchView = search？。actionView as SearchView :* 将搜索菜单项转换为 SearchView 类型的 actionView

*searchView？。apply {
isSubmitButtonEnabled = true
setOnQueryTextListener(this @ list fragment)
}*

在上面两行中，SearchView 小部件用于为非空搜索查询启用提交按钮并设置侦听器。

您已经成功地实现了 SearchView。