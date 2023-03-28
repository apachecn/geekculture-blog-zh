# 如何在下一篇 JS 中使用 Context，useReducer 和 LocalStorage

> 原文：<https://medium.com/geekculture/how-to-use-context-usereducer-and-localstorage-in-next-js-cc7bc925d3f2?source=collection_archive---------0----------------------->

![](img/a44e13ccd8c15792c853148723850c29.png)

Photo by [Christopher Gower](https://unsplash.com/@cgower?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

## 从应用内的任何位置访问状态，并在重新加载时保存数据

在任何类型的项目中，拥有一个可以从应用程序的任何地方访问的全局状态对象都非常方便。能够使用一个 reducer 并将所有内容保存在 LocalStorage 中甚至更有用。

为此，我们将查看 React useContext 和 useReducer 挂钩。

你要做的第一件事是在[https://nextjs.org/docs](https://nextjs.org/docs)创建一个包含非常好的文档的下一个 JS 项目。

我们将从上下文开始，然后是 Reducer，最后是 LocalStorage。

# 1.创建上下文

一旦你创建了一个基本的下一个 js 应用程序，你将不得不创建一个“上下文”文件夹，并添加一个“AppContext.js”文件。上下文是允许您与整个应用程序共享状态的部分，而“AppContext.js”文件将保存上下文和用于共享数据的包装。

我的 _app/
├─语境/
│ ├─ AppContext.js

首先，从 React 导入所有必要的钩子。

```
**import** { createContext, useContext, useMemo } from "react";
```

然后是 AppContext 变量来创建上下文和 AppWrapper，app wrapper 将上下文传递给应用程序的其余部分，并将从该文件导出。

```
**import** { createContext, useContext, useMemo } from "react"; **const** AppContext = createContext(); ***export******function*** AppWrapper({ children }) {***const*** [appState, setAppState] **=** useState({});***const*** contextValue **=** useMemo(() *=>* { ***return*** [appState, setAppState]; }, [appState, setAppState]);***return*** ( <AppContext.Provider *value*={contextValue}>
      {children}
   </AppContext.Provider> );}**export** *function* useAppContext() { ***return*** useContext(AppContext);}
```

context/AppContext.js

在 AppWrapper 中，我们定义了一个 AppState 状态变量，并使用 useState 挂钩对其进行初始化(这是状态数据所在的位置，然后可以使用 setAppState 方法对其进行修改)。使用 useMemo 钩子，我们可以记忆状态值以获得更好的性能。然后我们返回一个提供者，我们可以用它来输出带有上下文值的应用程序的其余部分。最后，我们导出 useAppContext 函数，然后我们将能够从应用程序中的任何地方导入该函数以获取状态数据。

现在，导航到“pages”和“_app.js”，在这里我们将导入我们的 AppWrapper，并用它包装我们的布局或应用程序。

my _ app/
├─context/
│├─app context . js
├─pages/
│├─_ app . js

```
**import** "@/styles/globals.scss";
**import** Layout from "@/components/Layout";**import** { AppWrapper } from "@/context/AppContext";**function** MyApp({Component, pageProps}) {
   **return** (
      <>
         <AppWrapper>
            <Layout>
               <Component {...pageProps} />
            </Layout>
         </AppWrapper>
      </>
   );
}**export** default MyApp;
```

pages/_app.js

一旦你这样做了，你就可以在你的应用程序的任何地方使用上下文了。

```
**import** { useAppContext } from "@/context/AppContext";**const** [appState, setAppState] = useAppContext();
```

anywhere/anycomponent

现在您已经完成了上下文，我们将能够添加 useReducer 挂钩来提高状态可用性。

# 2.添加 useReducer 挂钩

在“AppContext.js”旁边创建一个名为“AppReducer.js”的文件，它将包含一个 initialState 变量和一个 AppReducer arrow 函数。initialState 变量将用于存储应用程序的初始状态，arrow 函数将用于存储用于修改状态的调度方法。

```
**export** **const** initialState = {
   number: 0,
};**export** **const** AppReducer = (state, action) => {
   switch (action.type){
      case "add_number": {
         **return** {
            ...state,
            number: action.value + state.number,
         };
      }
   }
};
```

context/AppReducer.js

在本例中，initialState 存储一个值为 0 的“number ”, AppReducer 有一个名为“add_number”的调度方法，可以调用该方法，该方法通过使用该方法调度的任何值来增加状态中的数字。

现在我们已经有了 initialState 和 reducer，我们可以将它们导入到上下文中，并使用这两个参数将 useState 替换为 useReducer。(不要忘记也从 React 导入 useReducer 以便能够使用它)

```
***import*** { createContext, useContext, useMemo, useReducer } *from* "react";
***import*** { AppReducer, initialState } *from* "./AppReducer"; ***const*** AppContext **=** createContext();**export** *function* AppWrapper({ children }) { ***const*** { state, dispatch } **=** useReducer(AppReducer, initialState); ***const*** contextValue **=** useMemo(() *=>* {
      ***return*** { state, dispatch };
   }, [state, dispatch]); ***return*** (
      <AppContext.Provider *value***=**{contextValue}>
         {children}
      </AppContext.Provider>
   );} **export** *function* useAppContext() {
   ***return*** useContext(AppContext);
}
```

context/AppContext.js

现在，您已经将 useReducer 挂钩添加到了您的应用程序上下文中，使用它的过程类似于我们在应用程序中使用 useState 变量和方法的过程。只需导入 useAppContext 挂钩，而不是 AppState 和 setAppState 析构状态和调度。

```
**import** { useAppContext } from "@/context/AppContext";**const** { state, dispatch } = useAppContext();
```

anywhere/anycomponent

这就是你访问数据和方法的方式(更多关于 useReducer 钩子的信息，请访问[https://reactjs.org/docs/hooks-reference.html#usereducer](https://reactjs.org/docs/hooks-reference.html#usereducer)。

```
**const** { number } = state;dispatch({type: "add_number", value: 3)};console.log(number);
```

anywhere/anycomponent

现在，您可以访问您的数据，并可以在应用程序中的任何地方修改它，让我们添加一种方法，使它在重新加载后保持不变。

# 3.将您的数据保存在本地存储中

能够在重新加载页面后保持状态非常有用，例如，如果您想创建一个订餐应用程序，并且您想在访问另一个页面后保留您的购物篮。

我将使用 useEffect 钩子，另一个解决方案是创建一个自定义的 useLocalStorage 钩子，不幸的是，在钩子中使用钩子通常比它的用处更复杂。

第一步是从 AppContext.js 文件中的 React 导入 useEffect 钩子。

```
**import** {useEffect, useReducer, createContext, useContext, useMemo } from "react";
```

context/AppContext.js

现在让我们创建两个 useEffect 挂钩，一个检查本地存储中是否已经有一个状态(如果有，更新状态)，另一个用最新的数据更新本地存储。关于 https://reactjs.org/docs/hooks-effect.html 的[的更多信息。](https://reactjs.org/docs/hooks-effect.html)

```
useEffect(() *=>* { if (*JSON***.**parse(localStorage**.**getItem("state"))) { 

      *//checking if there already is a state in localstorage
      //if yes, update the current state with the stored one*dispatch({ 
         type: "init_stored", 
         value: *JSON***.**parse(localStorage**.**getItem("state")),
      }); }}, []);useEffect(() *=>* { if (state *!==* initialState) {

      localStorage**.**setItem("state", *JSON***.**stringify(state)); 

      *//create and/or set a new localstorage variable called "state"* }}, [state]);
```

context/AppContext.js

在第一个钩子中，我使用 dispatch 方法从 reducer 调用“init_stored ”,看起来像这样，它用存储的状态替换了整个状态。

```
**export** **const** initialState = {
   number: 0,
};**export** **const** AppReducer = (state, action) => {
   switch (action.type){ case "init_stored": {
         **return** action.value,
      } case "add_number": {
         **return** {
            ...state,
            number: action.value + state.number,
         };
      }
   }
};
```

context/AppReducer.js

搞定了。现在，您可以在应用的任何地方使用和更新共享状态，并且在重新加载时不会丢失:)

如果有任何错误/不连贯，或者如果你想让我写另一个主题，请告诉我！

因为这是我的第一篇文章，任何反馈都是受欢迎的！

# 来源:

*   上下文:[https://www . netlify . com/blog/2020/12/01/using-react-context-for-state-management-in-next . js/](https://www.netlify.com/blog/2020/12/01/using-react-context-for-state-management-in-next.js/)
*   图片来自 [Unsplash](https://unsplash.com/)
*   [碳](https://carbon.now.sh/)为代码嵌入