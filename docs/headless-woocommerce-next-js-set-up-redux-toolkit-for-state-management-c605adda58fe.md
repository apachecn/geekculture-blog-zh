# Headless WooCommerce & Next.js:为状态管理设置 Redux 工具包

> 原文：<https://medium.com/geekculture/headless-woocommerce-next-js-set-up-redux-toolkit-for-state-management-c605adda58fe?source=collection_archive---------14----------------------->

![](img/e8b221ee6629ef1dba6862e0938f1ac7.png)

Photo by [sarandy westfall](https://unsplash.com/@sarandywestfall_photo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在某些时候，我们必须处理国家管理。我认为`useState`钩子在大多数情况下都很棒，但是一直流行的 Redux 在不同的组件之间给了你更多的灵活性。在本文中，我将向您展示如何使用 Redux Toolkit 来管理购物车的状态。

我已经学会了喜欢 Redux，但开始的时候有点棘手。Redux store、reducers、actions、types、slices、dispatch、selector……有相当多的术语需要记住，但是 Redux 对于更大的项目特别有用，这也是我坚持的主要原因。

React `Context` API 是跨组件共享数据的另一种方式，无需在每一级手动传递属性。

在 [Redux 的官网](https://redux.js.org/introduction/getting-started)上，他们告诉你，“Redux Toolkit 是我们官方推荐的编写 Redux 逻辑的方法”。我认为最好遵循他们的建议，他们的 [Redux Toolkit 网站](https://redux-toolkit.js.org/introduction/getting-started)包含了很好的文档和教程。

# 安装 Redux 工具包

```
yarn add @reduxjs/toolkit react-reduxnpm install @reduxjs/toolkit react-redux
```

从 React Redux v7.2.3 开始，`react-redux`包依赖于`@types/react-redux`，因此类型定义将自动随库一起安装。否则，您需要自己手动安装它们(通常是`npm install @types/react-redux`或`yarn add @types/react-redux --dev`)。

# 配置商店

在其核心，我们需要建立一个 Redux 存储，这样我们就可以在任何功能组件中保存和检索数据。为此，我喜欢在根目录下创建一个`\store`文件夹，把所有东西放在一个地方。

你可以看到我们已经使用`configureStore`创建了一个商店，在这里我们可以将多个 reducers 传递到商店中，以保持事物的有序。把这些归约器想象成状态容器。每个容器本质上都是一个键值对对象，以您定义的结构存储数据。我包括了一个`cartReducer`，我们一会儿会谈到。

# 创建类型化挂钩

Redux 有`useDispatch`和`useSelector`钩子，分别帮助我们更新状态和检索状态。Redux Toolkit 建议创建`useDispatch`和`useSelector`钩子的类型化版本,以便在你的应用中更容易、更可靠地使用。

在`store.ts`文件中导出`RootState`类型和`AppDispatch`类型后，我们可以创建一个单独的`hooks.ts`文件，并创建`useDispatch`和`useSelector`的类型化版本。然后这些可以被导入到任何需要使用钩子的组件文件中。

# 创建冗余切片

我认为片的方式是 Redux 片允许我们管理状态容器。在上面的例子`store.ts`中，我将`cart: cartReducer`作为我的状态容器之一传入。对于每个状态容器，我们将希望定义它存储什么数据作为它的状态，并且还定义让我们改变状态的特定动作(例如，一个动作将数字计数器加 1，或者另一个动作切换布尔标志)。

Limited version of the cartSlice.ts

上面的例子给出了一个切片结构的概念，但是仍然需要更多的工作来定义 reducers 并将它们导出为 actions。

我们要做的第一件事是为我们的状态数据定义接口类型——我称之为`CartState`。对于我们的`CartState`,我现在只想存储一个`LineItem`数组。您可以添加更多数据，并根据需要进行定义。

现在我们知道接口已经被定义了，我们可以继续定义我们想要的初始状态。我想从一个空数组开始。您可以设置任何想要的默认值，只要它与您在接口中声明的类型相匹配。

接下来，我们用`createSlice`创建切片本身。在这里，我们可以给它一个名字，并通过我们刚刚定义的初始状态。然后，我们需要创建允许我们操纵状态的 reducers。然后用`cartSlice.actions`将这些减速器导出为动作。

最后，我们导出`cartSlice.reducer`，这样我们可以将它传递给`store.ts`。

# 在 Redux 切片中创建事例缩减器/操作

我必须承认，关于减速器和动作的术语确实让我困惑。在很长一段时间里，我习惯了动作是允许我们操纵状态的功能。现在在`createSlice`中，我们将 case reducers 作为允许我们操纵状态的函数。案例减少或行动？

因此，在`createSlice`的 reducer 参数中定义的 case reducer 函数将有一个相应的 action creator，并将包含在片的`actions`字段中。虽然这是一个繁琐的答案，但它帮助我认识到这两个术语是多么紧密地联系在一起。对我来说，把这些功能想象成动作更有用，因为这样描述起来更准确。

在我的例子中，我想创建几个操作来管理购物车状态，其中之一是添加一个行项目。我们可以直接在`cartSlice`中编写函数，但是我已经抽象了函数，所以如果函数的数量变得太大，我可以将它们重构到不同的文件中。

你可以看到`addLineItemReducer`有两个参数:`state`和`action`。`state`简单地允许函数访问当前保存在状态容器中的数据。`action`是一个类型化的`PayloadAction`，本质上保存了我们想要传递给这个函数的任何数据。在这个例子中，我们将有一个`LineItem`,当我们调用这个函数时，我们将把它作为一个参数传递给这个函数。

我想做的第一件事是检查我们在`action`有效负载中传递的`LineItem`是否已经存在于我们的状态中。我使用`.findIndex`来做这件事，如果它不在数组中，它返回-1，否则它给我们状态数组中`LineItem`的索引。剩下的逻辑是将`LineItem`添加到状态数组中，如果我们还没有它，如果我们有它，那么更新状态中那个`LineItem`的数量。

现在让我们用我们的`addLineItemReducer`来充实前面的`cartSlice`例子。

你可以看到我们已经把它作为`addLineItem`添加到`createSlice`中，并且我们导出这个动作函数以在其他组件中使用。

# 在组件中使用类型挂钩

我们可以使用我们最近创建的`useAppDispatch`钩子在我们选择的功能组件中分派`addLineItem`动作。

导入`useAppDispatch`和`addLineItem`。接下来，实例化`useAppDispatch`:

```
const dispatch = useAppDispatch()
```

为了分派`addLineItem`并将特定的行项目添加到购物车状态，我们可以使用类似下面的代码行，其中`lineItem`是一个`LineItem`对象。

```
dispatch(addLineItem(lineItem))
```

如果您想访问 Redux 商店和购物车 reducer/state，那么您可以导入`useAppSelector`并像这样使用它。

```
const cartState = useAppSelector((state) => state.cart)
```

在我们的例子中，`cartState`只保存了一个`LineItem`的数组，我们可以通过`cartState.lineItems`来访问它。

# 完成我的购物车切片

我想在我的`cartSlice`中添加更多的减少器/动作，这些是给你的信息。

[第 1 部分:用 WooCommerce 创建一个本地 WordPress】](https://leojbchan.medium.com/headless-woocommerce-next-js-create-a-local-wordpress-with-woocommerce-4411b24a160e)

[第 2 部分:设置 WooCommerce &测试与邮递员](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-woocommerce-test-with-postman-5e7859c65626)

[第 3 部分:设置 Next.js](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-next-js-b8d2193b2688)

[第 4 部分:用 TypeScript 和 Next.js 设置样式组件](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-styled-components-with-typescript-and-next-js-18cc047ccd79)

[第五部分:展示产品并创建订单](https://leojbchan.medium.com/headless-woocommerce-next-js-display-products-and-create-orders-593a6c5aee86)

[第六部分:建立 Redux 状态管理工具包](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-redux-toolkit-for-state-management-c605adda58fe)

[第 7 部分:创建购物车](https://leojbchan.medium.com/headless-woocommerce-next-js-create-a-cart-8a3e49b90076)

[第 8 部分:设置条带并创建检验](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-stripe-and-create-checkout-d01333607a66)