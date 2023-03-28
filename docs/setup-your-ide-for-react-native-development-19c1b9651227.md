# 为 React 本机开发设置 IDE

> 原文：<https://medium.com/geekculture/setup-your-ide-for-react-native-development-19c1b9651227?source=collection_archive---------10----------------------->

![](img/eb551cda2869b34584e51c033baba847.png)

很少有 ide 可以用来开发 React 本地应用程序。但是由于某些原因，React 原生社区的大多数人更喜欢 Visual Studio 代码，也就是 VSCode 。很明显，它是免费的，并附带了许多扩展，可以将您的 VSCode 转换为任何风格的开发。(也就是说，如果是 Java 开发，你可以安装一套扩展，让你在编码时更有效率。)

就像这样，我发现了一组扩展，根据我的工作经验，它们可以为 React Native 带来高效的软件开发。下面是我的 React Native 扩展包

注意:你可以使用我创建的这个 [**扩展包**](https://marketplace.visualstudio.com/items?itemName=shamendra.react-native-vscode-extensions-pack) 轻松安装下面所有的扩展包，而不用一个一个地安装。

**1。一个好的代码格式化程序:** [**更漂亮的**](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)当你使用 react-native CLI(使用 react-native init***your project name****)生成 react 原生项目时，你会得到一个更漂亮的配置文件(默认)。*但是你可以使用这里提到的[中任何一种更漂亮的配置文件格式](https://prettier.io/docs/en/configuration.html)，并且很容易地进行配置。
一旦你为 Mac 用户安装了快捷键，就会启用**‘shift+option+f’**组合键。它将捕获您的默认配置文件。但是有时你必须指向它，但是通过指出你的本地配置文件路径来编辑 VSCode 插件的设置。

**2。React 原生的调试和集成命令:** [**React 原生工具**](https://marketplace.visualstudio.com/items?itemName=msjsdiag.vscode-react-native)

![](img/6ba09cb1c31c8832ab457ff80d164d31.png)

如果我只是从官方页面复制描述的话，*“这个 VS 代码扩展为 React 原生项目提供了一个开发环境。使用这个扩展，您可以* ***调试您的代码，并从命令调板中快速运行*** `***react-native***` ***命令*** *。”*。你可以通过 gif 图片看到，微软的这个插件对 react 原生开发的支持。

**3。代码片段:**[**ES7 React/Redux/graph QL/React-原生片段**](https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets)

简单的扩展将为您提供一些非常有用的片段，以加速您在 JS/TS 和 ES7 中使用 React、React Native、Redux 和 Graphql 的开发。一旦你使用了这个插件，你就离不开它了。在我写这篇文章的时候，你已经安装了 2，563，798 个插件。作为对我上面解释的一点了解，你可以看看下面的视频。

**4。组件生成:**[**vs code React Native Component Generator**](https://marketplace.visualstudio.com/items?itemName=abdullahceylan.vscode-react-native-component-generator)发现这个组件生成器在为组件做大量演示/概念验证的情况下，以及在希望让每个人的代码在结构上更加一致的情况下非常有用。默认情况下，这将生成一个样式文件，我们在大多数时候并不需要它，因为把它放在组件下面，放在一个 js/jsx 文件中是很清楚的。但是如果它生成并保存在不同的文件中，这并不是一件坏事，因为在 Angular 中这是标准的约定。当我看到这个插件支持开箱即用时，我个人非常喜欢它。它将支持定制，你可以通过点击[插件页面](https://marketplace.visualstudio.com/items?itemName=abdullahceylan.vscode-react-native-component-generator)来查看。

**5。静态类型检查:** [**流语言支持**](https://marketplace.visualstudio.com/items?itemName=flowtype.flow-for-vscode)

看到了[丹阿布拉莫夫](https://medium.com/u/a3a8af6addc1?source=post_page-----19c1b9651227--------------------------------)给出的这个解释。现在学习 Typescript 对你的发展真的是一件好事。但是它也有利弊。因此，如果您的团队精通 Javascript，那么大多数情况下，Flow 将是一个不错的选择。
PropTypes 在 React 开发中很方便，但它只是在运行时对代码进行类型检查，也有一定的局限性。
安装此插件后，它将检测的默认流量配置。项目根目录下的 flowconfig 文件。所以你只需要在 VSCode 中安装这个插件，它就会自动运行。您可以通过[这个](https://flow.org/en/docs/config/)来添加和更改配置。

**6。IntelliSense:**[**Visual Studio IntelliCode**](https://marketplace.visualstudio.com/items?itemName=VisualStudioExptTeam.vscodeintellicode)您应该通过安装微软的此插件来启用智能感知功能。它会让你的生活比你想象的更舒适。

**如果你喜欢这个，**
👏请给我 5 个以上的掌声，这将有助于其他需要帮助的人接触到这篇文章。

如果你喜欢我的分机，请在这里评价。[https://marketplace.visualstudio.com/items?itemName = shamendra . react-native-vs code-extensions-pack # review-details](https://marketplace.visualstudio.com/items?itemName=shamendra.react-native-vscode-extensions-pack#review-details)

干杯！