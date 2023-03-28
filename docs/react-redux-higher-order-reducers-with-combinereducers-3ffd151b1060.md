# React + Redux:带有组合减速器的高阶减速器()

> 原文：<https://medium.com/geekculture/react-redux-higher-order-reducers-with-combinereducers-3ffd151b1060?source=collection_archive---------43----------------------->

![](img/8ea1c1336429e73ed105a6baac995965.png)

在 React 应用程序中使用 Redux 进行全局状态管理是一个彻底的游戏改变者——您不再需要有策略地在一个不断扩大(并且令人困惑)的本地状态网络中导航，以将正确的数据提供给正确的组件。但是伴随着强大的力量而来的是巨大的责任……比如坚持单一责任原则(也就是[中的 S 固体](https://en.wikipedia.org/wiki/SOLID))！在本文中，我们将介绍如何确保您的应用程序符合 [SoC](https://en.wikipedia.org/wiki/Separation_of_concerns) ，同时仍然利用 Redux 的强大功能。

我将用来演示这一点的示例应用程序是我最近为[熨斗学校](https://medium.com/u/973c5cbfb09b?source=post_page-----3ffd151b1060--------------------------------)的软件工程课程模块 5 完成的一个 [GIF 库应用程序](https://github.com/bfirestone23/gif-library)。该应用程序允许用户创建帐户/登录，搜索 GIPHY 的 API，并在他们创建的集合中添加/删除 gif。然后，用户可以复制该 GIF 的链接，与朋友分享，或者“喜欢”吸引他们眼球的收藏。基本上，这是一种用户友好的方式来组织你最喜欢的 GIF 并与朋友分享(因为我们总是在寻找任何给定时刻的完美 GIF，对吗？).

## 设置场景

当务之急是决定如何让你的减压器发挥作用。本质上，您应该问问自己——哪些 Redux 操作操纵了我的全局状态的哪一部分？以我的用户缩减器为例，让我们来看看你的目标是什么:

```
const user = (state = { status: 'pending' }, action) => {
    switch (action.type) {
        case 'setStatus':
            return { ...state, status: action.payload }
        case 'setUser':
            return { ...action.payload, status: 'resolved' }
        case 'noUser':
            return { status: 'idle' };
        case 'logout':
            return { status: 'idle' }
        default:
            return state;
    }
}export default user;
```

您会注意到，这个缩减器只影响它可以访问的 Redux 状态部分，反之亦然。足球之神很高兴！

现在，冲洗并重复其余的 Redux 动作。我的应用程序利用了收藏、gifSearch 和用户缩减器——注意这些是如何与单个应用程序功能保持一致的，从而使应用程序保持坚实和直观的结构。

## 减少减速器

所以你已经把你的 reducer 分开了(理想的是在“reducer”文件夹中，duh)并且问你自己，“好的，那么我们如何把这些和 Redux 存储连接起来？”。

首先，在 reducers 文件夹中创建一个新的`index.js`文件。在这里，您需要导入每个单独的缩减器，加上 Redux 库中的 combineReducers 函数，就像这样:

```
import gifSearch from './gifSearch';import collection from './collection';import user from './user';import { combineReducers } from 'redux';
```

接下来，我们将定义一个变量`rootReducer`(确保它被导出，这样我们就可以在我们的`store.js`文件中访问它)并将它设置为`combineReducers`的值，每个单独的缩减器都被传入:

```
export const rootReducer = combineReducers({ collection, gifSearch, user })
```

## 把所有的放在一起

现在是时候把我们的`rootReducer`挂在我们的商店了——最后一程。你的`store.js`文件应该看起来像这样，一旦说了和做了(我使用的是 Redux devtools Chrome 扩展，它是`compose()`函数的可选的第二个参数):

```
import { createStore, compose, applyMiddleware } from 'redux';import thunk from 'redux-thunk';import { rootReducer } from './reducers/index';export function configureStore() { return createStore( rootReducer, compose( applyMiddleware(thunk), window.__REDUX_DEVTOOLS_EXTENSION__ && window.__REDUX_DEVTOOLS_EXTENSION__() ) );}export const store = configureStore();
```

现在，假设您的存储被传递到您的`<Provider>`组件中，您已经准备好了！

不过，在退出之前还有最后一点需要注意——无论您在哪里通过 mapStateToProps 访问状态，您现在都需要指定您正在访问哪个缩减器，如下所示:

```
const mapStateToProps = state => {
  return { 
    collections: state.collection.collections,
    activeCollection: state.collection.activeCollection,
    status: state.user.status,
    username: state.user.username 
  }
}
```

有了它，你的 Redux 状态不再是由几十行代码组成的令人费解的混乱——随着你的应用不断增长，你的应用、你的同事和你的大脑都会为此感谢你。这篇文章只是触及了表面，为了更深入地理解像`combineReducers`这样的功能在做什么，一定要查看一下 [Redux 文档](https://redux.js.org/recipes/structuring-reducers/using-combinereducers)。