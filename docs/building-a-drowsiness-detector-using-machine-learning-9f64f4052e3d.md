# 使用机器学习构建睡意检测器

> 原文：<https://medium.com/geekculture/building-a-drowsiness-detector-using-machine-learning-9f64f4052e3d?source=collection_archive---------13----------------------->

## 这种困倦检测方法快速、高效且易于实现。

![](img/7708e41e0645a80a80890a46ba945c16.png)

Photo by [Kalegin Michail](https://unsplash.com/@kalegin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

我们将构建一个计算机视觉应用程序，它能够使用带有 OpenCV、Python 和 dlib 的面部标志来检测视频流中的睡意。为了建造这个探测器，我们将使用*眼睛长宽比*(耳朵)，介绍…