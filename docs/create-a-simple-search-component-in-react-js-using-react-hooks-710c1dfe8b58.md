# 使用 React 钩子在 React.js 中创建一个简单的搜索组件

> 原文：<https://medium.com/geekculture/create-a-simple-search-component-in-react-js-using-react-hooks-710c1dfe8b58?source=collection_archive---------0----------------------->

## 关于在 React 中使用钩子构建动态搜索组件的简单教程

![](img/ac921414b8628b9540e350c62e6e40de.png)

Photo by [Arnold Francisca](https://unsplash.com/@clark_fransa?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

任何网站最重要的组成部分之一就是它的搜索部分，它能立刻让用户体验变得更好，如果稍微用心一点，它也能把网站变成一个优秀的设计杰作。

在本文中，我将制作一个搜索 react 组件，您可以轻松地将其集成到自己的 React web 应用程序中。搜索组件，即使有时看起来有点复杂，也是一个相当简单的组件，尤其是如果你懂一点 React 的话。

> *当我第一次想做自己的搜索组件时，我不得不查看大量的文章，这些文章太复杂了，而且对一个简单的组件来说是不必要的复杂。*
> 
> *所以，我将带你通过这几个简单的步骤，在 React 中自己制作一个搜索组件，并在最后提供一个 Github repo。*

# 设置项目

## 先决条件

1.  `Node.js`安装在您的系统上
2.  `create-react-app`软件包已安装
3.  优选地，IDE
4.  愿意在搜索组件上工作

## 创建 React 应用程序

首先，我们将在`create-react-app`包的帮助下创建一个 React 应用程序，因此我们运行以下命令:

```
npx create-react-app react-search
cd react-search
```

在这之后，我们还需要`[tachyons](http://tachyons.io/)`(使用`tachyons`不是必须的，但是如果你不想在这里设计组件的样式)，所以我们使用 yarn 添加它，命令如下:

```
yarn add tachyons
```

现在，您已经准备好开始开发 React 应用程序了。

## 清理杂物并设置应用程序(可选)

现在，您已经创建了您的应用程序，在 IDE 中打开您的工作文件夹，您会看到许多文件已经存在，但对于这个项目(通常是所有小规模项目)的范围，我们可以删除许多这些文件，使您的文件夹看起来更好一些。因此，删除项目的`src`文件夹中的这些文件:

1.  `App.test`
2.  `App.css`
3.  `index.css`
4.  `logo.svg`
5.  `serviceWorker.js`
6.  `setupTests.js`

这将清空您的`src`文件夹，您所有的工作都将在这里完成。

现在，打开你的`public`文件夹，转到`index.html`，在文件中，将你的应用的`title`更改为 React Search(或者你喜欢的任何东西)，即将`<title>React App</title>`替换为`<title>React Search</title>`,并在上面添加下面一行，以便在你的应用中使用`[tachyons](http://tachyons.io/)`:

```
<link rel="stylesheet" href="https://unpkg.com/tachyons@4.12.0/css/tachyons.min.css"/><title>React Search</title>
```

在上面的链接中`tachyons`的版本可能会改变，你可以从[这里](http://tachyons.io/#getting-started)复制最新的命令。

现在，打开`src`文件夹中的`App.js`和`index.js`文件，删除不必要的行，之后它们应该是这样的(虽然`React.StrictMode`是可选的):

index.js

App.js

但是请注意，这是一个可选步骤，如果你不想从头开始创建一个应用程序，你可以省去这个步骤。

## 创建数据文件

这里有两种方法，这完全取决于你希望你的应用程序如何工作。

第一个是在`App.js`文件本身中添加带有`useState`钩子的数据。如果我们创建一个从用户那里获取数据的表单，这可能是一个好方法(否则在`App.js`中会有太多的混乱)，但是为了本文的简单，我们将选择另一种方法。

第二种方法是创建一个不同的文件，然后将其导入到`App.js`文件中，从而获得一个清晰的外观。

因此，我们从在`src`文件夹中创建一个`data`文件夹开始，在这个文件夹中我们创建一个名为`initialDetails.js`的文件。现在，该文件将包含一个列表，该列表又包含详细信息。

我们用 5 个主要细节来描述一个人。对于图片，我使用了来自[无限设计](https://limitlessdesigns.io/)的 8 位头像插图，你也可以从[这里](https://limitlessdesigns.io/avatar-illustrations/)免费获得(可能需要注册)。然后，我在`public`文件夹的`assets`文件夹中创建了一个名为`img`的文件夹，并在`img`文件夹中随机粘贴了五张图片，按照给定的格式命名为 1-5。

任何给定人员的`imgPath`变量将是来自`public`文件夹的相对地址(我们将在后面看到如何使用这个 imgPath，因为 React 不允许使用`src`文件夹之外的任何项目)。

由于在这里创建数据是一项琐碎的任务，我建议您将下面给出的代码复制粘贴到您的文件中，并根据需要进行必要的更改。

initialDetails.js

完成这一部分后，我们最终可以继续在 React 中创建实际的应用程序。

# 从项目开始

现在，我们终于准备好工作了，所以我们首先要做的是在`src`文件夹中创建一个名为`components`的新文件夹。这里，我们将创建 4 个新文件，即:

```
1\. Card.js        // Card component to display details
2\. SearchList.js  // Component to list out the cards
3\. Scroll.js      // Component for making the list scrollable
4\. Search.js      // Main search component
```

## Card.js

我们将从`Card`组件开始，它只是将一个人的详细信息放在一个卡片组件中，使它看起来更好。

卡片组件将接收一个人的详细信息，然后显示出来。

Card.js

这里，在第 7 行中，我们使用了`tachyons`来制作一张卡片，在第 8 行中再次将图像变成一个圆形头像。接下来，我们使用`process.env.PUBLIC_URL`来访问`public`文件夹中的所有文件，然后将`imgPath`变量连接到它，以访问这个人的图像。之后，我们简单地显示一个人的细节。

## SearchList.js

接下来，我们看一下`SearchList`组件，顾名思义，它在`map`函数的帮助下构建这些`Card`组件。

SearchList.js

首先，我们导入`Card`组件，然后我们对从 render 部分的父组件获得的过滤列表使用`map`函数。通过这个 map 函数，我们传递所需的参数，为每个人呈现不同的卡片。

之后，在返回部分，我们调用之前创建的`filtered`对象。

## Scroll.js

现在，如果像这样使用`SearchList`组件，随着数据的增加，它将占用整个屏幕的空间，为了反对这种情况，创建了一个`Scroll`组件，它也相当简单且不言自明。

Scroll.js

在这个组件中，其内部的组件以`70 viewport height`的高度呈现，如果溢出，则将其变成 y 轴的可滚动组件。

## 搜索. js

最后，我们到达`Search`组件，从文章的角度来看，这是最重要的组件。首先，我们从`react`导入`useState`。然后，我们导入`Scroll`和`SearchList`组件。接下来，在`Search`函数中，我们使用`useState`钩子用`useState("")`(一个空字符串)初始化`searchField state`的值。此后，我们在从父节点接收的`details`列表上使用`filter`函数。

在这个过滤函数中，我们检查两个值，人的名字和他们的电子邮件，然后用`toLowerCase`函数把它们转换成小写，之后我们用`includes`函数检查搜索栏是否包含细节中的任何字母。如果它包含指定的查询，则特定人员的详细信息被发送到`filteredPersons`。

Search.js

在这之后，我们转到 return 并为组件创建一个标题和一个用于搜索细节的输入框。所有这些在`tachyons`包的帮助下很容易被风格化。在输入中，我们将`onChange`属性的值作为`handleChange`函数。

`handleChange`功能依次用`setSearchField()`设置`searchField`的值。

现在，为了渲染所有的细节，有点类似于我们在`SearchList`组件中所做的，我们创建一个函数(看位置，函数要在 return 之外创建)。该函数将`SearchList`组件包装在`Scroll`组件中，并在内部传递`filteredPersons`对象。稍后，在返回中调用这个函数来呈现搜索查询的结果。

这就完成了我们的`Search`部分，因此也是文章的主要部分。接下来，我们继续到`App.js`来完成这个`Search`组件。

## App.js

我们之前编辑了这个组件，使它看起来很干净。因为我们已经为我们的`Search`功能创建了不同的组件，所以在集成它的时候，我们在`App.js`内部会有更少的混乱。

> 一个干净的 App.js 通常意味着一个创建良好的 web 应用。

首先，我们从`Search.js`文件导入`Search`组件，然后从`initialDetails.js file.`导入`initialDetails`列表

App.js

然后，我们使用`tachyons`来设置 web 应用程序的样式，并且用之前导入的`initialDetails`来填充`Search`组件的细节参数。

至此，我们完成了创建搜索组件的旅程。

# 运行我们的网络应用

最后，我们使用以下命令在 bash/命令提示符下运行我们的 web 应用程序:

```
yarn start
```

瞧，现在你有了我们自己的网络应用程序和一个工作搜索组件。

# 演示

如果您一直关注这篇文章，那么您应该已经创建了一个类似于下图所示的搜索框，显然欢迎您进行更好的设计，因为我主要关注的是文章中的结构、组织和工作部分，但是一个简单的搜索组件应该是这样的。

Demo for React-Search Web App

# 附加部分:隐藏`SearchList` 组件直到被调用

你并不总是希望所有的搜索结果都显示在你的网站上，所以为了隐藏它，除非在里面输入了什么，我们简单地做了三件小事，用`useState`创建一个新的状态`searchShow`,并将`false`作为初始值传递给它。

```
const [searchShow, setSearchShow] = useState(false);
```

然后，我们在`e.target.value`的帮助下检查来自搜索栏的输入是否为空，在这种情况下，我们将这个`searchShow`状态设置为`false`，否则我们在`handleChange`函数内的`setSearchShow`的帮助下将其设置为`true`。

```
if(e.target.value===""){
  setSearchShow(false);
}
else {
  setSearchShow(true);
}
```

在此之后，我们向`searchList`函数添加一个`if`语句，该语句检查`searchShow`是否为真，如果为真，则呈现`SearchList and Scroll`组件，否则不呈现。

```
if (searchShow) {};
```

看一下下面给出的新的`Search.js`文件，我上面提到的三个变化发生在第 10、29–34 和 38 行(第 44 行也是)。

Search.js

添加这五行将有效地隐藏您的`SearchList`组件，并且您的 web 应用程序在更改后应该是这样的。

看起来很酷，不是吗？自己尝试一下，如果你遇到任何问题，请随时联系我或查看我的 [Github Repo](https://github.com/shashankcic/react-search) 。