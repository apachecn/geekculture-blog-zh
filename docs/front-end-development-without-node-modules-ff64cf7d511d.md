# 没有节点模块的前端开发

> 原文：<https://medium.com/geekculture/front-end-development-without-node-modules-ff64cf7d511d?source=collection_archive---------7----------------------->

曾几何时，我们可以简单地将一个 HTML 和一个脚本文件放入 FTP 服务器，很快就有了一个工作网站，然后就可以收工了。

今天，我们不得不经历重重困难，只为在正确的地方得到正确的东西。假设爱丽丝想用她的周末做一个简单的待办应用程序或任何她喜欢的小创意。首先，她必须安装一大堆 10k npm 软件包文件。然后，她花了几个小时搜索如何让本周的 js bundler 使用最新的 typescript 和最新的趋势 UI 框架。当事情不工作或那些文章刚刚过时时，会很沮丧。一旦她实际上开始为她的小有趣的应用程序建立第一个功能，周末几乎已经过去了！

但是事情正在改变…

![](img/b19b88b5b9f0e6e885c7ee829fcc27c2.png)

Does Alice have to stay late on Sunday night to enjoy making her little fun app? (photo: [Paras Katwal](https://www.pexels.com/@paras?utm_content=attributionCopyText&utm_medium=referral&utm_source=pexels))

# 1.一点背景知识:CommonJS 和 ES 模块

使用 NodeJS，我们都熟悉 CommonJS，这是 NodeJS 加载依赖代码的标准(read: legacy)方式。在安装一个模块之后，比如说`lodash`，我们可以通过使用`require('lodash')`将它加载到我们的代码中。NodeJS 从一开始就是这样处理依赖关系代码的:

ECMAScript 2015 (ES6)推出了 ES Module——一个官方的、标准化的 JavaScript 模块系统。到这里花了一段时间。如今，[各大浏览器](https://caniuse.com/es6-module)和 NodeJS ( [从 v13.2.0](https://nodejs.medium.com/announcing-core-node-js-support-for-ecmascript-modules-c5d6dc29b663) 开始)默认发货支持 ES 模块。ES 模块具有静态分析、树抖动和异步的优点。

在 NodeJS 中，要启用 ES 模块，我们有两种选择:使用**。mjs** 扩展或设置**包中的`"type": "module"`. JSON**。虽然大多数开发工具都理解 ES 模块，但是仍然有许多不兼容的地方。例如，TypeScript 仍然不支持输出到。mjs 文件。或者 Vercel [与 es 模块](https://github.com/vercel/next.js/issues/9607)不兼容。所以仍然需要一些 transpilers 和变通方法。希望这种情况会很快改变。

NodeJS 中的许多包已经附带了 es 模块文件。但是很多包都不是。在撰写本文时，在 npm 上的[前 10 个依赖包中，只有](https://www.npmjs.com/browse/depended) [tslib](https://www.npmjs.com/package/tslib) 通过将`"exports"`包含在 *package.json* 中来支持 [ES 模块文件](https://github.com/microsoft/tslib/blob/e7a115533a28b90e48139e77462e0b5812983847/package.json#L25)。许多其他顶级软件包仍然不提供 es 模块:lodash，moment，react，request，axios，chalk，commander，express...这对于 NodeJS 来说实际上不是问题，因为 NodeJS 允许使用 [import 来处理 ES 模块和 CommonJS 格式](https://nodejs.org/api/esm.html#esm_interoperability_with_commonjs)。

但是浏览器没有这个特权。想在浏览器中导入自己喜欢的节点模块怎么办？嗯，你得走运。在撰写本文时，[React 在浏览器中入门的推荐方式](https://reactjs.org/docs/getting-started.html#add-react-to-a-website)是在`<script>`标签中包含 UMD 版本，并使用全局变量`window.ReactDOM`:

没有爱丽丝的 ES 模块。

# 2.Skypack

Skypack 是一项出色的 CDN 服务，transpiles node 将其打包，以便能够在浏览器中很好地工作。它得到了扫雪队的支持。只需将包 **name@version** 放在 **cdn.skypack.dev** 之后，就可以开始了:

就是管用！当然，你可能会问，我们可以进口“T21”牌 lodash-es 。但是许多包裹没有它们的二重身。或者它们不经常更新。这里， [skypack.dev](https://skypack.dev) 来帮忙了。

尽管如此，它仍然存在一些问题。由于不清楚的原因，一些版本不工作。游览[cdn.skypack.dev/react@16](https://cdn.skypack.dev/react@16)时，改为 React 版本 17。但是未来是光明的。Alice 现在可以开始在她的应用程序上工作，而不用花费她周末的大部分时间来配置本周的 js bundler …

***边注*** : *我也把自己的版本放在了*[*espkg.vercel.app/react@16*](https://espkg.vercel.app/react@16)*。你可以使用*[*espkg . vercel . app*](https://espkg.vercel.app)*作为替代，直到 Skypack 修复问题。其他的包也可以，比如 espkg.vercel.app/lodash@4 的*[](https://espkg.vercel.app/lodash@4)**(给它一点构建的时间，然后响应会被 Vercel 缓存)。**

# *3.积雪场*

*好吧，我撒了点谎。 *TypeScript* 在浏览器中直接不起作用。你仍然需要更多的工作。这就是 [Snowpack](https://snowpack.dev) 的真正威力:最小配置和远程包。你甚至不需要安装 *node_modules* 就可以开始使用你的小程序。只需运行两个安装命令:*

```
*yarn global add snowpack
snowpack init*
```

*这会给你一个空骨架 **snowpack.config.js** 。然后在`packageOptions`下增加单线配置`source: 'remote'`:*

*仅此而已！现在运行`snowpack dev`，开始添加你的**index.html**和 **myscript.ts** (没错，就是*打字稿*):*

*就是管用！🎉看马，没有 *node_modules！*没有 *package.json！我们甚至免费得到了*打字稿*和*热重装*。耶！**

# *概述*

*示例代码可以在这里下载:[gist.github.com/olvrng](https://gist.github.com/olvrng/e9729d550cb5c2e023ed0c1f8290978f)。在 [snowpack.config.js](https://www.snowpack.dev/reference/configuration) 上还有你可能需要的其他配置。让我们把那个留到下一天。现在开始修补你的应用程序，把你宝贵的时间花在最有价值的功能上吧！🚀🚀*

# *附言*

*哦，不过爱丽丝想少用[](http://lesscss.org/)**。别担心，只是一个单行配置…我发誓！她可以添加一个文件 **mystyle.less** 和多一行 **snowpack.config.js** 。一切都会好的。嗯，这次她一定要记得跑`yarn add snowpack-plugin-less`！只是这次...***

***感谢您的阅读！还有别忘了我的小页面 [espkg.vercel.app](https://espkg.vercel.app/) 。***

****先前发表于*[*olvrng . github . io*](https://olvrng.github.io/w/2021.02.02.snowpack/)*。****