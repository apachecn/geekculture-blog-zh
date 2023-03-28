# 让你编写的下一个 React 博客网站更容易。使用降价文件。

> 原文：<https://medium.com/geekculture/make-the-next-react-blog-site-you-code-easier-use-markdown-files-8e2d3b684dfe?source=collection_archive---------14----------------------->

## 为 markdown-to-jsx 动态加载降价文件

![](img/a815a37421cee481a197181992d9e544.png)

# 我正忙着如何在我的一个 React 组件中显示副本。

而不是处理 HTML **divs** 、**段落**、**图片**、*** *表格**** 等。

我找到了一种方法来导入**。md** ( *markdown* )文件我已经有了。

## 这一过程包括:

*   [**markdown-to-jsx**](https://www.npmjs.com/package/markdown-to-jsx)
*   对反应状态管理和生命周期了解不多；我们将在这里使用**钩子**
*   对 JavaScript **Fetch API** 了解很少(非常少的知识，不要担心)

# 我们需要做的第一件事是整理减价文件。

我已经选择储存**。 **src** 目录下 *markdown* 文件夹中的 md** 文件。

![](img/275f2bffb25a7dfe12030d86a4020bc2.png)

# 下面是将所有内容整合在一起的代码。

下面是我将要解释的代码:

```
// App.jsimport React, { useState, useEffect } from ‘react’;import Markdown from ‘markdown-to-jsx’;import ‘./styles/main_styles.css’; function App() { const file_name = ‘react_pinterest_clone.md’; const [post, setPost] = useState(‘’); useEffect(() => { import(`./markdown/${file_name}`) .then(res => { fetch(res.default) .then(res => res.text()) .then(res => setPost(res)) .catch(err => console.log(err)); }) .catch(err => console.log(err)); }); return ( <div className=”container”> <Markdown> {post} </Markdown> </div> );}export default App;
```

## **这里发生了 4 件关键的事情:**

1.  导入 **markdown-to-jsx** 包。

2.设置状态。

3.获取并显示降价。

**首先是**，我们当然需要导入 **markdown-to-jsx** 包。我们在返回街区使用它。

**其次**，我们设置我们将用来持有**的状态。md** 数据。最初，我们将变量 **post** 设置为一个*空白字符串*，并将其放置在 **Markdown** 标签之间。

我们还有一个**文件名**来动态选择我们想要的降价文件。既然这样

我对它进行了硬编码，但是您可以根据一些逻辑将其设置为您想要的任何值。

**第三**，一旦我们的组件加载， **useEffect()** ，我们使用 ****import**** 作为函数。

以这种方式使用的**导入**充当*承诺*并将绝对路径返回给我们的 markdown 文件。

然后我们使用**获取 API** 获取我们想要的 **markdown** 文件。

获取文件后，我们需要将响应解析为一个**文本**文件，然后将解析后的响应存储在我们的 **post** 状态变量中。

# **就这么简单。**

你可以在这里 得到源文件 [**。**](https://github.com/an-object-is-a/reactjs-markdown-dynamic-file)

如果你想要更深入的指导，可以看看我在 YouTube 上的完整视频教程， [**一个物体是一个**](https://www.youtube.com/c/anobjectisa) 。

一定要在 [**Instagram**](https://www.instagram.com/an_object_is_a/) 和 [**Twitter**](https://twitter.com/anobjectisa1) 上关注我们，及时了解我们最新的 **Web 开发教程**。

## 为您的博客动态加载降价文件到 React | Markdown-to-jsx