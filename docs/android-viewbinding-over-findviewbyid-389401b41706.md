# Android:findViewById()上的 ViewBinding

> 原文：<https://medium.com/geekculture/android-viewbinding-over-findviewbyid-389401b41706?source=collection_archive---------9----------------------->

![](img/ee288bf87f6ad80bbf470443a569010b.png)

Photo by [Artem Beliaikin](https://unsplash.com/@belart84?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

作为 Android 开发人员，我们都使用过 *findViewById()* ***，*** *(它在我们的布局 xml 文件中查找具有给定 id 的第一个后代视图)，*它使用 *getId()* 方法在视图层次结构中查找与 Id 匹配的视图。

*findViewById()* 可以正常工作，但是有一些问题:

**Null-Crashes** :如果找到了，getId()方法返回视图，如果 Id 无效或者在视图层次结构中没有找到，则返回 Null。

**类型安全**:findViewById()访问的视图不是类型安全的，可能会调用*findViewById<ImageView>(r . id . text viewname)，*或任何类似的不匹配，导致类强制转换异常。

**视图绑定**允许我们更容易地编写与视图交互的代码。一旦在模块中启用了视图绑定，它就会为该模块中的每个 XML 布局文件生成一个*绑定类*。绑定类的实例包含对在相应布局中具有 ID 的所有视图的直接引用。

在大多数情况下，视图绑定取代了 *findViewById。*

**在模块中启用视图绑定:**

视图绑定是在模块基础上启用的，因此要在特定模块中启用它，请将 *buildFeatures 下的 *viewBinding 字段*设置为 true。*

```
android {
    ...
    buildFeatures {
        viewBinding true
    }
}
```

> 重新构建项目，将为项目中的所有布局文件自动生成绑定类。

在 *viewBinding* enabled 模块中的每个绑定类都包含对根视图和所有带有 ID 的视图的引用。生成的绑定类的名称是通过将 XML 文件的名称转换为 Pascal 大小写并在末尾添加单词“binding”来创建的。例如:activity_main.xml 生成的绑定类将被称为 *ActivityMainBinding* 。

让我们举个例子来理解视图绑定的实现。

考虑 user_profile.xml

```
<ConstraintLayout ... > <TextView android:id="@+id/txtName" />
    <Button ... />
    <TextView android:id="@+id/txtProfile"/></ConstraintLayout>
```

生成的绑定类将被称为 UserProfileBinding。这个类有两个 TextView 字段:txtName 和 txtProfile。布局中的按钮没有 ID，所以在绑定类中没有对它的引用。

为了直接访问布局的根视图，每个绑定类都提供了一个 getRoot()方法。在这种情况下，getRoot()返回 ConstraintLayout 视图。

**在活动中设置视图绑定**

```
private lateinit var _binding: UserProfileBinding

override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    binding = UserProfileBinding.inflate(layoutInflater)
    val view = binding.root
    setContentView(view)
}
```

上面的代码片段调用生成的 binding 类中包含的 static inflate 方法并创建 binding 类的一个实例，通过调用 binding.root 方法访问根视图(这个调用 *getRoot()* 方法)，最后将根视图传递给 *setContentView。*

**在片段中设置视图绑定**

```
private var _binding: UserProfileBinding? = null

private val binding get() = _binding!!

override fun onCreateView(
    inflater: LayoutInflater,
    container: ViewGroup?,
    savedInstanceState: Bundle?
): View? {
    _binding = UserProfileBinding.inflate(inflater, container, false)
    val view = binding.root
    return view
}

override fun onDestroyView() {
    super.onDestroyView()
    _binding = null
}
```

在片段的情况下，inflate()方法要求用户传入一个布局充气器，而不是像在活动中那样进行绑定。

现在，访问视图和设置视图非常简单:

```
binding.txtName.text = "Anubhav"
binding.txtProfile.text = "Android Developer"
```

最后，如果您不希望为特定的布局文件生成绑定类，请使用工具名称空间添加 viewBindingIgnore，并将其设置为 true。

```
<ConstraintLayout
        ...
        tools:viewBindingIgnore="true" >
        ...
</ConstraintLayout>
```

那是我们的新朋友给你看的。