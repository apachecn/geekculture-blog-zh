# Headless WooCommerce & Next.js:用 TypeScript 和 Next.js 设置样式化组件

> 原文：<https://medium.com/geekculture/headless-woocommerce-next-js-set-up-styled-components-with-typescript-and-next-js-18cc047ccd79?source=collection_archive---------3----------------------->

![](img/8c489516ff3400148784e676c1b8a075.png)

Photo by [Marcus Ganahl](https://unsplash.com/@marcus_ganahl?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这真的应该比我认为的要容易！我喜欢使用样式化组件，为了让它与 Next.js 和 TypeScript 配合得更好，您需要配置一些东西。我认为有一篇文章专门介绍这种流行组合的正确设置会很有用。

# 安装依赖项

如果你使用的是 JavaScript，那么常规的依赖就可以了，但是如果你使用的是 TypeScript，那么你还需要安装类型来避免错误(取决于你有多严格)。

依赖关系:

```
yarn add styled-components
```

devDependencies:

```
yarn add babel-plugin-styled-components --dev
yarn add @types/styled-components --dev
```

# 巴比伦式的城市

[styled-components 支持带有样式表再水合的并发服务器端呈现](https://styled-components.com/docs/advanced#server-side-rendering)，这很重要，因为 Next.js 使用服务器端呈现。为了可靠地工作，你需要使用样式组件的巴别塔插件；因此将其添加为开发依赖项。添加后，您需要在根文件夹中创建一个名为`.babelrc`的新文件，配置如下:

# 自定义“文档”和谷歌字体

我们需要添加一个自定义的`/pages/_document.tsx`，然后使用样式化组件的逻辑将服务器端呈现的样式注入到`<head>`中。styled-components [有一个例子](https://github.com/vercel/next.js/tree/master/examples/with-styled-components)你可以跟随，但是它并不适合 TypeScript。

然而，在我向你展示我的定制`_document.tsx`文件之前，我想掩盖一下我是如何扩展它的，这样我就可以一次性地展示这个文件。

再往前看一点，我知道我想包括谷歌字体和 Next.js(从 v10.2 开始)已经内置了网页字体优化。我们需要做的就是在`<head>`中加入一个`<link>`。您可以使用自定义“文档”将字体添加到特定页面或整个应用程序中。考虑到这一点，我想扩展自定义的`_document.tsx`来包含这个特性。

这里是我使用的自定义`_document.tsx`文件，包括`DocumentContext`和`DocumentInitialProps`类型。

# 全局样式

老实说，我们已经准备好了，但我喜欢包含一个全局样式文件，它利用了[样式组件的帮助函数](https://styled-components.com/docs/api#createglobalstyle)，所以可以应用 CSS 重置或基本样式表之类的东西。我喜欢在根目录下创建一个`\styles`文件夹，并添加一个名为`globalStyles.ts`的文件。在这里你可以添加你想要全局应用的 CSS(例如，从 H1 标签中移除边距)。

将这个组件放在`_app.tsx`中 React 树的顶部，当组件被“渲染”时，全局样式将被注入。

# 主题

您可能希望您的项目有一个主题，而 styled-components 通过导出一个提供所有 styled-components 都可以访问的主题对象的`<ThemeProvider>`包装器组件，提供了[完整的主题支持](https://styled-components.com/docs/advanced#theming)。

有不同的方法来使用主题化支持，我在保存在`\styles`文件夹中的文件`theme.ts`中创建了一个主题对象。据我所知，您可以创建任何您喜欢的 JSON 对象，并且您将能够在您的样式化组件中访问这些属性。

现在，在`_app.tsx`中，我们需要用`<ThemeProvider>`包装我们的应用程序，并将主题传递给组件。

从这里开始，我们可以在样式化组件中访问我们的主题对象。

# 使用主题

在一个快速演示中，我剥离了`index.tsx`文件中的默认样板代码，并删除了所有冗余的代码、导入和文件。我想做的就是使用从`<ThemeProvider>`中获得的主题对象显示一个样式化的 H1 标签。

```
<StyledH1>Welcome to Next.js!</StyledH1>
```

然后[在组件的渲染方法之外](https://styled-components.com/docs/basics#define-styled-components-outside-of-the-render-method)我们定义了样式化组件。

```
const StyledH1 = styled.h1` font-family: ${(props) => props.theme.font.heading};`;
```

在我们的主题文件中，我在字体下指定了两个属性(`heading`和`body`)，所以如果你把属性改为`body`，你应该会看到不同的字体。您可能需要重新启动服务器才能看到更改。

[第 1 部分:用 WooCommerce 创建一个本地 WordPress】](https://leojbchan.medium.com/headless-woocommerce-next-js-create-a-local-wordpress-with-woocommerce-4411b24a160e)

[第 2 部分:设置 WooCommerce &测试与邮递员](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-woocommerce-test-with-postman-5e7859c65626)

[第三部分:设置 Next.js](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-next-js-b8d2193b2688)

[第 4 部分:用 TypeScript 和 Next.js 设置样式组件](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-styled-components-with-typescript-and-next-js-18cc047ccd79)

[第 5 部分:展示产品并创建订单](https://leojbchan.medium.com/headless-woocommerce-next-js-display-products-and-create-orders-593a6c5aee86)

[第 6 部分:为状态管理建立 Redux 工具包](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-redux-toolkit-for-state-management-c605adda58fe)

[第 7 部分:创建购物车](https://leojbchan.medium.com/headless-woocommerce-next-js-create-a-cart-8a3e49b90076)

[第 8 部分:设置条带并创建检验](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-stripe-and-create-checkout-d01333607a66)