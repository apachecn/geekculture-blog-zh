# Python 中的变形图像

> 原文：<https://medium.com/geekculture/deforming-images-in-python-66e0d0dcb17f?source=collection_archive---------11----------------------->

使用枕头变形功能

![](img/53de5901cb53447e2896feb604f0dcdc.png)

Photo by [Dan DeAlmeida](https://unsplash.com/@ddealmeida?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

您可以使用 Pillow ImageOps `deform`功能对图像进行常规变形。典型变形包括:

*   桶形失真。当图像中心比边缘看起来更大时，就会发生这种情况。它有时被称为鱼眼效应。
*   枕形失真，与桶形失真相反。