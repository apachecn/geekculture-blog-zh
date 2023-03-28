# 所有前端开发者都应该知道的 Webpack 插件

> 原文：<https://medium.com/geekculture/2-webpack-plugins-all-frontend-developers-should-know-b8706f23b627?source=collection_archive---------53----------------------->

![](img/e9d4875a523ecd9a1f899adc85b7d818.png)

Photo by [Jorge César](https://unsplash.com/@jorgecesar?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

使用捆绑器对于现代 UI 应用程序来说是必不可少的

Webpack 是所有 UI 捆绑器的 OG，随着新 UI 库的引入，现在需要捆绑您的代码以供所有浏览器理解并创建优化的产品版本。

在 web 开发中，曾经有一段时间包含一个脚本标签就足以将您最喜欢的 JavaScript 库加载到您的 web 应用程序中，无论是 JQuery 还是任何其他类似的库，但是现在 UI 更加复杂和强大。你再也不能实现同样的极简应用程序来满足你的客户，所以你必须添加大量的库，即使你不想增强体验或提高开发速度。

React、angular 和 Vue 以及其他 UI 播放器依赖于某种直接或内部提升的第三方库捆绑器，例如 Angular 自带 Webpack 实现来捆绑开箱即用的代码，react 使用 create-react-app 来处理复杂性。

尽管如此，还是有必要了解捆绑器的基本功能，我们手头的主题中的 Webpack，以及它的一些插件是如何使用的，它们提供了什么好处。

# **你应该知道的 Webpack 插件**

## 1.Webpack 定义插件

很多时候，我们需要在应用程序中创建全局变量。我们可以在后端使用环境变量来实现这一点，但由于 UI 没有任何这样的奢侈，这个 Webpack 插件试图模仿相同的概念，并为您的项目创建一个全局定义的变量，让您能够在**编译时**处理不同的环境及其特定配置。

样本:

```
new webpack.DefinePlugin({
  PRODUCTION: JSON.stringify(true),
  VERSION: JSON.stringify('5fa3b9'),
  BROWSER_SUPPORTS_HTML5: true,
  TWO: '1+1',
  'typeof window': JSON.stringify('object'),
  'process.env.NODE_ENV': JSON.stringify(process.env.NODE_ENV),
})
```

用法:

```
if (!PRODUCTION) {
  console.log('Debug info');
}

if (PRODUCTION) {
  console.log('Production log');
}
```

类似地，我们可以在编译时根据这些变量，从你的。env 文件，否则您的 UI 应用程序在运行时将无法访问它。

我会怎么做

```
if (PRODUCTION) {
  console.log = function() {}
}
```

这段代码所做的是在你的产品中，也许你错过了或者你的团队错误地提交了一些不需要的控制台，这些不会打印出来。

## 2.HTML Webpack 插件

这是使用 Webpack 时的另一个重要插件，假设需要将动态命名的脚本注入到您的 HTML 文件分发中，以提供给客户。

如果你还没有弄明白，这就没那么容易了。

**你可以给出静态名称，但这会导致浏览器不必要的缓存你的脚本资产，你需要做一个硬刷新，并告诉你的客户做同样的事情，这可能会很尴尬。**

我如何处理这个问题，在 Webpack 中你有一个输出键。

在这里您可以定义您的输出文件以及 Webpack 应该编译和保存资产的发行版。

```
output: {
        filename: '[name].[contenthash].js',
        path: path.resolve(__dirname, '../dist'),
        publicPath: "/"
    },
```

这个内容散列会在每次构建时改变，并且您不会面临**缓存问题。**

如何使用这个插件

```
new HtmlWebpackPlugin({
      path: path.join(__dirname, "dist"),
      template: path.join(__dirname, "public/index.html"),
      inject: true
    }),
```

这将告诉 Webpack 将 web 应用程序的动态包注入到哪个文件中，不管它自动注入的名称是什么，您需要提供的只是一个模板(一个基础文件),它是一个用作模板的文件，新生成的包入口点被注入到该文件中。

使用这段代码，它将使用一个模板键来复制基本文件和路径，以输出到发行包中。

除了这个特性之外，HTML Webpack 插件还有很多其他的特性，用于不同类型的构建，你可以删除注释，缩小代码，修改你的应用程序。

## 荣誉提名:

TerserPlugin(web pack V5 中不需要)，CompressionPlugin，MiniCssExtractPlugin

市场上还有一些其他新的捆绑软件值得关注，Snowpack、Vite 很快为自己创造了一个名字，但就功能集而言，webpack 仍然是一个更强大的捆绑软件。

快乐编码，不断学习，保持好奇心。