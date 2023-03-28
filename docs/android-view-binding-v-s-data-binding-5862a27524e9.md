# Android:视图绑定 v/s 数据绑定

> 原文：<https://medium.com/geekculture/android-view-binding-v-s-data-binding-5862a27524e9?source=collection_archive---------20----------------------->

![](img/b26e127c771268fe86c24e9ebb9a42ef.png)

Photo by [Pierre Bamin](https://unsplash.com/@bamin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在上一篇文章中，我谈到了在 findViewById()上使用视图绑定的好处。我强烈建议你在阅读这篇文章之前先阅读一下[](/geekculture/android-viewbinding-over-findviewbyid-389401b41706)**。**

**在这篇文章中，我将讨论数据绑定，视图绑定和数据绑定之间的主要区别，以及何时应该在项目中使用它们。**

**假设您已经学习了视图绑定，我将直接跳到数据绑定，它的实现，然后讨论视图绑定和数据绑定之间的区别。**

****数据绑定****

> **数据绑定库是一个支持库，允许您使用声明性格式而非编程方式将布局中的 UI 组件绑定到应用程序中的数据源。**

**若要启用数据绑定，请在模块级 build.gradle 文件中添加以下代码段:**

****启用数据绑定**:**

```
android **{ 
    ...** buildFeatures **{** dataBinding true
    **}
}**
```

****生成绑定类****

**与视图绑定不同，在视图绑定中，一旦启用了视图绑定，就会为项目中的所有布局文件自动生成绑定类，而使用数据绑定，您必须显式地将每个布局转换为数据绑定布局才能生成数据绑定类。**

**要将布局转换为数据绑定布局，请转到父布局并按下 *Option + return* 按钮，同时将打开一个菜单。从出现的菜单中选择*转换为数据绑定布局*和您的视图将被转换为数据绑定布局。**

```
<layout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto">
    <data>
        <variable
            name="mainViewModel"
            type="com.anubhav.data.ViewModel" />
    </data>
    <ConstraintLayout... /> <!-- UI layout's root element -->
</layout>
```

**可以在表达式中使用的绑定变量是在一个数据元素中定义的，该数据元素是 UI 布局的根元素的同级。这两个元素都包装在布局标记中。之后，您可以使用已定义的变量，通过声明性格式为 UI 元素设置值。**

****绑定数据****

**为每个布局文件生成一个绑定类。默认情况下，类的名称基于布局文件的名称，将其转换为 Pascal 大小写并添加后缀*绑定*。比方说，上面的布局文件的名称是 activity_main.xml，因此相应的生成类是 ActivityMainBinding。该类保存从布局属性到布局视图的所有绑定，并知道如何为绑定表达式赋值。创建绑定的推荐方法是在展开布局的同时进行，如下例所示:**

```
class MainActivity : AppCompatActivity() { private val mainViewModel by *viewModels*<MainViewModel>() override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        val binding: ActivityMainBinding = DataBindingUtil.setContentView(this, R.layout.*activity_main*)

        binding.mainViewModel = mainViewModel
    }

}
```

**现在在布局文件中，**

```
<layout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto">
    <data>
        <variable
            name="mainViewModel"
            type="com.anubhav.data.ViewModel" />
    </data>
    <ConstraintLayout... >         <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            **android:text="@{mainViewModel.name}"**/> </ConstraintLayout>
</layout>
```

**如果在 Fragment、ListView 或 RecyclerView 适配器中使用数据绑定项，则应使用 BindingUtil 类或 DataBindingUtil 类的 inflate 方法，**

```
val listItemBinding = ListItemBinding.inflate(layoutInflater, viewGroup, false)// orval listItemBinding = DataBindingUtil.inflate(layoutInflater, R.layout.list_item, viewGroup, false)
```

**现在可能存在这样的情况，您不能使用声明格式直接为视图设置属性，例如，您可能想通过 url 将图像加载到 ImageView 中，或者根据特定结果设置视图的颜色。数据绑定为我们提供了绑定适配器来适应这样的场景。**

****绑定适配器****

**绑定适配器负责进行适当的框架调用来设置值。每当绑定值改变时，生成的绑定类必须用绑定表达式调用视图上的 setter 方法。您可以允许数据绑定库自动确定方法、显式声明方法或提供自定义逻辑来选择方法。通常的绑定适配器方法只是一个静态方法，为布局文件中的特定自定义属性执行数据转换和注入。一个常见的绑定适配器类看起来像这样，**

```
class DataRowBinding {

    companion object {

        @JvmStatic
        @BindingAdapter("loadImageFromUrl")
        fun **loadImageFromUrl**(view: ImageView, imageUrl: String) {
            view.*load*(imageUrl) **{** placeholder(R.drawable.*ic_error_image*)
                crossfade(600)
                error(R.drawable.*ic_error_image*)
            **}** }
    }
```

**自定义属性的名称定义如下:**

> ****@ binding adapter(" loadImageFromUrl ")****
> 
> ***现在您可以在****@ binding adapter(" loadImageFromUrl ")**内使用这个属性*，**loadImageFromUrl****来设置布局元素内的值。我喜欢将方法的名称与我定义的自定义属性的名称保持一致。******
> 
> *******@JvmStatic*******
> 
> ******指定如果它是一个函数，那么需要从这个元素生成一个额外的静态方法。如果这个元素是一个属性，应该生成额外的静态 getter/setter 方法。******

*****现在，您可以使用绑定适配器中定义的自定义属性，*****

```
***<layout xmlns:android="http://schemas.android.com/apk/res/android"
        xmlns:app="http://schemas.android.com/apk/res-auto">
    <data>
        <variable
            name="mainViewModel"
            type="com.anubhav.data.ViewModel" />
    </data>
    <ConstraintLayout... ><ImageView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            **loadImageFromUrl="@{mainViewModel.imageUrl}"**/></ConstraintLayout>
</layout>***
```

*****如果您在绑定类中使用一个 **LiveData** 对象，您需要指定一个生命周期所有者来定义 LiveData 对象的范围。*****

```
***val *binding*: ActivityMainBinding =   DataBindingUtil.setContentView(this, R.layout.main)// Specify the current activity as the lifecycle owner.**binding.setLifecycleOwner(this)*****
```

*****你可以走了。*****

*********视图绑定** **和** **数据绑定**的区别*******

*   *******视图绑定库比数据绑定库**快**，因为它没有利用底层的注释处理器，而且就编译时速度而言，视图绑定更加**高效**。*******
*   *****视图绑定的唯一功能是绑定代码中的视图，而数据绑定提供了更多选项，如**绑定表达式**，它允许我们编写表达式，将变量连接到布局中的视图。*****
*   *****数据绑定库与**可观察数据**对象一起工作，你不必担心当底层数据改变时刷新用户界面。*****
*   *****数据绑定库为我们提供了**绑定适配器**。*****
*   *****数据绑定库为我们提供了**双向数据绑定，**这是一种将对象绑定到 xml 布局的技术，这样对象和布局就可以相互发送数据。*****

> *****简而言之，没有什么是视图绑定能做的，也没有什么是数据绑定做不到的，(尽管代价是更长的构建时间)，而且有很多是视图绑定做不到的。*****

*****事实上，您可以在同一个项目中使用这两个库，高级用例使用数据绑定，普通用例使用视图绑定。*****

*****这就是所有的人，希望你今天学到了一些东西！*****