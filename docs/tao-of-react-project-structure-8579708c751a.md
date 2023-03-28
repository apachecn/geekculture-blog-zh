# 反应之道——项目结构

> 原文：<https://medium.com/geekculture/tao-of-react-project-structure-8579708c751a?source=collection_archive---------0----------------------->

我记得当我第一次开始钻研它的时候，我被它如何完全改变了 web 应用程序开发的游戏惊呆了。

配备一个**小** API，让一个**快速**掌握它。我很快开始参加黑客马拉松，这是我以前从未做过的事情，并且一直排在前三名，这是我的武器库中的新武器。

![](img/cf1c2dc9e51a9565b44bd295c93dc06f.png)

Tao can loosely translate as “The Way”

有许多不同的方式来构建 react 应用程序，我喜欢这种自由，但这也使它成为一把双刃剑，因为它是如此的不独立。

我**不是**声称这里解释的结构优于其他方法，但它只是一种简单的 ***方法*** ，我发现在构建应用程序时，它是非常**可扩展的**，尤其是在处理同一个项目中的**多个团队成员**时。

> 这篇文章将解释一个你可以应用的方法，一个模板，如果你愿意，你可以塑造它来满足你的需要。因为没有针对每个项目的明确的“一刀切”的解决方案。

# 现实与期望

网上的大多数指南在谈到反应时，都会描述它的**特性**，以及如何**利用**它们。我没有在网上看到很多深入的指南，关于如何在创建一个你计划**扩展**的**现实世界**应用时应用它。

大多数指南遵循一种方法，即完全基于"**文件类型**来组织文件。"当您的应用程序在您知道它之前开始增长时，您留下的是一个**难以管理的应用程序**，尤其是有**多个**团队成员和特性的应用程序。

![](img/ef08b9548c7752c4873d13eeecdd75ce.png)

Application structures can quickly get out of hand

# 让文件夹来说话

我喜欢遵循一种"**基于特征的"**方法。

当我们写代码时，我们是在写一个**故事**，就像我们用其他语言写一样。我坚信同样的方法可以应用于应用程序的构造方式。

我还做了一个**样例** Github repo，可以作为**参考**[https://Github . com/xXValhallaCoderXx/Tao-react-app-structure](https://github.com/xXValhallaCoderXx/tao-react-app-structure)

> **看应用结构，应该能明白是什么样的应用。**

作为一个高级示例，让我们看看下面的结构

```
/src
    index.js
    root-routes.js (Top level routes)
    /pages
        /profile
        /products
            index.js (Nested Routes for Products)
            /detail
                index.js (Container for Detail)
                Detail.js
           /list
               index.js (Container for List)
               List.js
               ComponentA.js
    /shared
        /styles
        /utils
        /components
            /atoms
            /molecules
            /organisms
        /templates
```

> 这大概是你的项目应该嵌套的部分，下面我们将对每个部分做一个简要的分析。

## **index.js**

只是您的标准入口点，以连接 React 到 DOM 并导入您的应用程序所需的任何文件。

## **root-routes.js**

顶层路由是在你的应用程序中定义的，
这个文件**不需要**所有底层嵌套路由的上下文。

```
<Route>
  <Switch>
    <Route path="/profile" component={Profile}/>
    <Route path="/products" component={Products} />
  </Switch>
</Route>
```

## **/页面/产品/索引. js**

在这些文件中，我们将为应用程序的每个页面或特性提供**嵌套路由**。

```
<Switch>
  <Route eaxct path="/products" component={List}/>
  <Route exact path="/products/:id" component={Detail} />
</Switch>
```

## **/页面/产品/列表/索引. js**

在大型应用程序中，您很可能会使用某种形式的状态管理，如 Context API、Redux、Mobx 等。无论您选择使用哪种方法，都应该保持**与应用程序的视图层**分离。

我们将这些文件视为我们的 ***【容器】*** 组件，它将**将**连接到我们正在使用的任何状态管理，**准备****数据**，以及所需的**功能，并将其向下传递到**表示层**。**

将来，如果你曾经**改变**状态管理库，大部分的**重构**将会在**应用程序的这一部分**中。只要同样的道具传下来，你的表示层就不会受到影响。

## **/页面/产品/列表/列表. js**

作为一种命名惯例，我喜欢将每个“特性”的“**顶层** l”表示层命名为特性本身。允许轻松辨别**容器**和根**呈现**层之间的**差异**。

这个文件可以使用一个模板组件，允许应用程序之间的一致性，特别是如果有许多相似的不同页面。

## **/页面/产品/列表/组件 A.js**

您可能仍然希望将表示层分成小的组件，因此您仍然可以在应用程序的这一部分中添加特定于功能的组件。

**注意**
在更大的特性中可能需要将这个 ComponentA.js 放入它自己的文件夹中，并作为一个容器连接到一个商店，如果它使你的应用程序更容易管理，没有什么可以阻止你这样做。

# 共享文件夹

这个文件夹是我们保存文件的地方，这些文件可能会在应用程序的多个部分中使用，比如样式、实用程序甚至是我们的存储状态片段。

![](img/3097a3040dc69b61b734c85dd846ec79.png)

Be nice — let others know where to find you

## **/共享/风格**

这一部分应该是不言自明的，你所有的全局样式表都将是活的，SCSS 混合等。

## **/共享/实用工具**

实用函数可以放在这个文件夹中，我们可以在这里进行解析、验证、包装库等等。

## **/共享/组件**

可重复使用的组件保存在这里。当涉及到构建组件时，我喜欢使用 Brad Frosts 的原子设计方法。如果这个教程得到了很好的反馈，我可能会写一篇续篇，讲述我如何将它融入到我的应用程序中。

## **/共享/模板**

模板组件将位于该文件夹中，例如，如果每个页面都在顶部使用导航栏，您可以有一个“主”布局，或者如果您的编辑页面具有与您的应用程序一致的特定设计，“编辑”布局将位于此处。

我喜欢在构建模板组件时使用**复合模式**，我觉得它允许在使用组件时有更多的灵活性，比如改变某些东西呈现的顺序，但仍然保持 UI 所需的一致性。

## **/共享/切片**

“**切片**是我们在整个应用程序中共享的应用程序状态的部分，所有的**业务逻辑**、访问**的方式**、**状态**都将存在于这个部分中。

在这里，我将使用 Redux 作为我的状态管理的例子，但是您可以**将**与**相同的原则**应用于您喜欢的任何其他形式的状态管理。

如下例所示，
用户片将存储关于登录用户的所有数据，提供选择器通过应用程序访问该状态，并提供 sagas 处理 API 调用以获取或编辑与用户相关的数据。

对于 toast 切片，尤其是使用单例模式时，控制 Toast 通知的所有逻辑都将位于共享文件夹中。

```
/shared/slice
    /user
       /index.js - Barrel file to export relevant methods
       /selectors.js - State selectors (permissions, roles, info)
       /actions.js - Actions to trigger in the application
       /reducer.js - Store reducer to deal with user state
       /saga.js - Business logic related to user
    /toast
       /index.js
       /selectors.js - Get toast notifcation state (open / closeed)
       /actions.js - Actions to control the toast notification
       /reducer.js
```

![](img/9e7b9ac42bfc59894a9cab8023faa3b2.png)

Just like the Yin Yang — We keep our application layers separate, mixing a bit of each layer

**注**

当从一个 API 接收到数据时，我喜欢把它直接放入 reducer，我使用选择器来解析数据。

原因是在应用程序的不同部分，相同的数据可能需要以不同的方式使用**。所以我们应该保持从 API 接收的数据格式，然后**使用适当命名的选择器**解析数据，并用于在**表示层**显示数据。**

> **应用程序的 UI 层及其大部分业务逻辑应该与您选择使用的状态管理库无关。**

## **GQL 呢？**

**随着 GQL 也变得越来越流行，方法仍然是一样的，您将在您的**容器(index.js)** 文件中为每个**特性**获取数据。**

**这些 GQL 模式可以存在于您各自的特性**“切片”**文件夹中，如果它们是特定于特性本身的，或者如果它是将在多个地方使用的东西，您可以将它们放在适当的**“切片”**文件夹中，这些文件夹存在于**共享**文件夹中。**

**使用这种方法，只要你保持数据向下传递到你的表示层**相同**，你就必须做**最小的重构。**例如，如果您正在**将您的 API 调用从 **Redux** 切换到由 **GQL** 处理，并且可以随着时间的推移**慢慢完成这个迁移**。****

## **这是所有的乡亲**

**这是一个较高层次的小介绍，介绍了我如何构建 React 应用程序的规模。**

**我创建了一个小的**例子**回购[这里](https://github.com/xXValhallaCoderXx/tao-react-app-structure)可能有助于一些教程更容易理解和跟随，它只是一个简单的**例子来提供更多的上下文。****

**根据反馈，我可能会继续分享我在使用 React 期间发现的其他有用的技巧和方法。**

**希望你喜欢这个指南！**

**保重，祝编码愉快！**