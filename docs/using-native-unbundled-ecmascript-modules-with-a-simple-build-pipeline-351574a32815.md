# 通过简单的构建管道使用本机、非捆绑的 ES6 模块

> 原文：<https://medium.com/geekculture/using-native-unbundled-ecmascript-modules-with-a-simple-build-pipeline-351574a32815?source=collection_archive---------10----------------------->

![](img/81ea936bef1b81c335b4c5d646c6a63f.png)

我们的旅程开始于大约七年前，当时 YUI3 被雅虎砍掉。

我们的应用程序在从开发到生产的每一个环境中都使用了 ECMAScript 模块(也称为 ES6 模块或 ESM)。没有捆绑，代码分块/分裂，或其他复杂的构建设置。

# 介绍

我们的应用程序是一个服务器端渲染的多页面应用程序。它有大约 50-100 个不同的页面，取决于你如何计数。许多这样的页面都有专门的 JS 控制器。大多数还为 UI 组件(例如下拉菜单、标签视图)、实用程序(例如事件委托)和其他东西使用共享模块。

这些模块中的大多数(控制器、组件、实用程序等)都依赖于其他模块，并且这些依赖关系可能会重叠。

让我们看一个例子。

```
// controller.js
import Stateful from '../util/stateful.js';

import('../components/panel.js').then(mod => new mod.default());

// panel.js
import Stateful from '../util/stateful.js';
```

顺便说一句，您可能已经注意到模块名称以`../`开头！你可能已经习惯在[里看到`import mod from 'name';`你的](https://v8.dev/features/modules) [最喜欢的](https://exploringjs.com/es6/ch_modules.html#sec_overview-modules) [学习](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import) [资源](https://www.geeksforgeeks.org/how-to-use-an-es6-import-in-node-js/)。但是在浏览器中，模块名*必须*是绝对或相对路径，所以*必须以*、`../` 或`/`开头。(继续在你的控制台中键入`import('foo');`，看看会发生什么。我会在这里等。)

## 本机模块加载

我们的应用直到最近还在使用 [YUI3 的模块加载器](https://clarle.github.io/yui3/yui/docs/yui/#dynamic-loading-with-use)，我想找一个好的替代品。

那么 ES6 模块加载在浏览器中是如何工作的呢？

嗯，看看[文档的状态](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import#syntax)，[我在火狐](https://bugzilla.mozilla.org/show_bug.cgi?id=1656248)发现的这个 bug(很快得到修复)以及缺乏教程和工具，你会认为这根本不可行，或者至少是最前沿的。根据谷歌 Chrome 的平台状态，[不到 4%的页面负载使用它](https://www.chromestatus.com/metrics/feature/timeline/popularity/2062)！

我发现的大多数[资源都使用捆绑器](https://engineering.velocityapp.com/webpack-vs-browersify-vs-systemjs-for-spas-95b349a41fa0)，比如 [Webpack](https://webpack.js.org) ，将你所有的代码捆绑到一个文件中，传输到 ES5，并在此过程中加入任何需要的 polyfills。

我也试过 [SystemJS](https://github.com/systemjs/systemjs) ，它是/应该是一个用于导入的 polyfill。我发现与一起工作非常困难，即使是琐碎的例子。

幸运的是，这些都没有必要，至少不再需要了！ [ES6](https://caniuse.com/es6) 和[导入](https://caniuse.com/mdn-javascript_statements_import)在所有浏览器上都得到[支持(除了 IE，很明显——很好的摆脱)。](https://v8.dev/features/modules)

## 捆扎机和单页应用程序

如今，大多数应用似乎都是 SPAs，可用的库(React、Vue 等)和构建工具(Webpack、Rollup 等)反映了这种优势。

对我来说，将所有的复杂性都转移到客户端是没有意义的，即使我们可以从头开始。大本营的[开发者同意](https://turbo.hotwire.dev/handbook/introduction)。

将我们所有的 Javascript 代码捆绑到一个巨大的`app.js`中也没有意义。对任何代码的任何更改都意味着需要完整的构建。

从技术上来说，web pack[支持多页面应用](https://webpack.js.org/concepts/entry-points/#multi-page-application)，但是看看他们对这项技术的描述有多少。创造 50 个以上的入口点似乎是一个绝对的噩梦。不要介意添加一个页面，忘记添加一个入口点，只是在生产中才发现。

# 挑战

我想在 ES6 模块中编写代码，让事情变得简单。

没有捆绑或分块，没有构建代理在后台持续传输文件，没有为开发和生产创建和维护复杂的构建架构。请不要这样！

理想情况下，我们应该在 IDE 中编写 ESM，并在开发中一字不差地使用这些文件。为了制作，我们显然会缩小。

# 解决方案:本机 ESM 已准备好投入生产

幸运的是，在开发中使用原生 ESM 是微不足道的！

你也可以一字不差地把它交付生产，它也能正常工作。显然，你至少应该缩减代码。但是我们这么做已经很多年了，所以设置起来也很琐碎。

看，这个解决方案非常简单，但是非常有效。

为什么没人推荐这个？

# 性能怎么样

嗯，有一个重要的问题，那就是生产中的性能。(谷歌的 V8 团队在这个话题上有一本[非常好的入门](https://v8.dev/features/modules#performance)。去读吧。)

例如，考虑这个依赖链

```
controller  -> tabview
  tabview     -> buttongroup, widget
    buttongroup -> button, widget
      button      -> widget
    widget      -> stateful
    stateful    -> attribute, eventTarget
```

浏览器需要依次下载这些文件*。例如，直到它已经加载了`controller`，它才知道它需要`tabview`。(糟糕的是,`rel="modulepreload”`的支持[很差,](https://caniuse.com/?search=modulepreload)HTTP/2 push 的配置[很难,](https://v8.dev/features/modules#http2),因为这些可能会有所帮助。)*

*YUI 的模块加载器通过向客户端发送完整的依赖图解决了这个问题。然后，它可以计算出需要哪些模块，并通过一个请求获取所有模块。整洁！*

*现代的`import`不可能做到这一点，因为模块名必须是静态的——解析不能依赖于任何正在运行的代码。你连`import foo from 'foo' + '.js';`都做不到！*

*(实际上，前面的语句只适用于`import` *语句*。你可以用这种方式改进 [*动态*导入](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import#dynamic_imports)，但是不值得。你很快就会明白为什么。)*

*HTTP/2 和缓存在这里帮了很大的忙，但是浏览器仍然必须请求每个模块*，然后*才能开始下载它的依赖项。*

## *内容哈希缓存-崩溃*

*缓存破坏是提高资产下载性能的最常见的方式。通过给每个资产版本一个永久的 URL，我们可以让浏览器(代理、cdn 等)知道他们可以暂时(甚至无限期地)使用该资源的缓存版本，甚至不需要请求 URL。*

*所需要的是类似于*

```
*<FilesMatch "\.\w{8}\.js$">
  Header set Cache-Control "public, max-age=3133337, immutable"
</FilesMatch>*
```

*这完全解决了性能问题，因为浏览器从磁盘加载整个依赖链。*

*这种方法非常依赖热缓存。如果你的应用有很多使用冷缓存的用户，这种方法可能不适合你。*

## *履行*

*如果我们能动态地将模块名改为`import foo from './foo.r2d2.js'`，我们所有的问题都将迎刃而解。*

*我很惊讶我找不到任何解释如何做到这一点的博客、谈话或插件。*

*所以我决定自己试一试。*

*事实证明，编写一个向模块名添加内容哈希的 [Babel 插件非常简单(多亏了](https://www.npmjs.com/package/babel-plugin-add-contenthash-to-imports)[精彩的文档](https://github.com/jamiebuilds/babel-handbook/blob/master/translations/en/plugin-handbook.md)):*

```
*// from
import foo from './foo.js';
import('../bar.js');// to
import foo from './foo.r2d2.js';
import('../bar.c3p0.js');*
```

*你甚至不需要重命名文件。只需将您的 web 服务器配置为忽略内容哈希:*

```
*RewriteRule ^(.+)\.\w{8}\.js$   $1.js  [last]*
```

# *结论*

*如今在开发中使用 ESM 真的很容易。你不需要 transpilers，bundlers，polyfills，什么都不需要！(如果你真的还需要支持 IE，可以考虑另谋高就。我们正在招聘！)*

*显然，生产环境中的 ESM 同样简单，但我们确实需要做些事情来提高性能。*

*幸运的是，Babel 插件非常容易实现，并在生产中产生非常快的性能。*

*结果也是非常缓存友好的，因为我们只使实际上已经改变的文件无效。(如果您捆绑文件，如果任何文件的*发生变化，您需要重新创建整个捆绑包。真是浪费！)**

*由于我们的 l10n 设置，我们的应用程序确实有点复杂，但这是另一个故事了。*

*我真的希望这能帮助一些人。如果那个人是你，请和我分享你的经历！*