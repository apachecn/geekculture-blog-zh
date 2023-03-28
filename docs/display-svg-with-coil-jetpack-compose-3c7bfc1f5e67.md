# 显示带线圈的 SVG-Jetpack 组合

> 原文：<https://medium.com/geekculture/display-svg-with-coil-jetpack-compose-3c7bfc1f5e67?source=collection_archive---------5----------------------->

![](img/b8918c845f3942670a4cc542dd06c893.png)

作为一名 android 开发人员，加载 SVG 图像是一项日复一日的任务。

使用 Coil 库，这项任务变得非常高效，因为 Coil 自动处理网络请求、转换和缓存。

如果你想知道更多关于线圈库和它是如何工作的，参考这篇博文[这里](/@_pundirAbhishek/jetpack-compose-image-loading-using-coil-647a8098c217)。

1.  **导入依赖关系**

在 build.gradle 文件中，为线圈库添加这些依赖项。

(届时版本可能会有所不同，你可以从[这里](https://coil-kt.github.io/coil/compose/)查看。)

```
// Coil Library Dependencies
implementation "io.coil-kt:coil-compose:1.3.2"
implementation "io.coil-kt:coil-svg:1.3.2"
```

2.**获取 SVG Url**

Coil 有一个 SVG 解码器，帮助我们在组件中显示 SVG 图像。

假设我们有一个 SVG 链接。

```
val svgLink = "https://restcountries.eu/data/aus.svg"
```

3.**图像加载器**

现在我们需要定义一个图像加载器。

`ImageLoader`是执行`[ImageRequest](https://coil-kt.github.io/coil/image_requests/)`的[服务对象](https://publicobject.com/2019/06/10/value-objects-service-objects-and-glue/)，它们处理缓存、数据获取、图像解码、请求管理、位图池、内存管理等等。

```
val imageLoader = ImageLoader.Builder(*LocalContext*.current)
    .componentRegistry **{** add(SvgDecoder(*LocalContext*.current))
    **}** .build()
```

4.**获取图像**

然后，无论何时我们想要使用它，我们都必须将它传递给 CompositionLocalProvider。在里面你定义一个画家，其余的是经典构图。

```
*CompositionLocalProvider*(*LocalImageLoader* provides imageLoader) **{** val painter = *rememberImagePainter*(svgLink)

    *Image*(
        painter = painter,
        contentDescription = "SVG Image",
        modifier = Modifier
                     .size(80.dp)
                     .*clip*(*CircleShape*),
        contentScale = ContentScale.FillBounds
    )
**}**
```

这里我们看了如何在我们的组件中显示 SVG 图像，只需要一个 SVGDecoder。

接下来，我们将探索一些其他与 Jetpack Compose 相关的特性。

**到那时快乐作曲。**