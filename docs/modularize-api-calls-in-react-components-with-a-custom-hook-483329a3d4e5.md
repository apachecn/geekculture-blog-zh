# 用自定义钩子模块化 React 组件中的 API 调用

> 原文：<https://medium.com/geekculture/modularize-api-calls-in-react-components-with-a-custom-hook-483329a3d4e5?source=collection_archive---------8----------------------->

当我学习构建 React 组件时(我在骗谁呢，我还在学习)，我记得能够在我的组件中构建简单的 API 调用的快乐。我会导入一个像 [Axios](https://www.npmjs.com/package/axios) 这样的库，编写一个简单的 HTTP 函数，并为数据响应设置一个状态片段。我会为这种简单性感到高兴，然后继续编写下一部分功能。

![](img/27ff2d50a7ea9fe82d9a9db38c8d6c73.png)

然而，没过多久，我就意识到，这些散落在代码库中的特殊 API 调用，以及一季社区的风格一致性，将比 2021 年比特币对 Elon 推文的价格反应更快地让位于代码熵。

我想到的答案是利用脸书在 2018 年宣布的钩子架构来构建一个模块化的定制钩子，抽象 API 调用。我设想了一个代码库，开发人员不用担心编写 API 调用。相反，他们会利用一个记录良好的 API 调用库，并使用带有 setter 函数的一行钩子来触发组件中的调用。这样，API 调用将在整个代码库中实现标准化，开发人员将专注于编写简洁、强大的组件，而自定义 API 挂钩将充当整个应用程序的引擎。

下面是它如何工作的详细解释。但是如果你对着你的显示器大喊大叫，“省省吧，给我看看代码！”，那你就更有力量了。[这是 GitHub](https://github.com/dgamboa/useapi-hook) 上的代码。

**设计挂钩**

当定义钩子的功能需求时，我有四个想法:

1.  钩子应该通过返回一个带有响应对象和 setter 函数的数组来镜像 setState
2.  钩子应该包含存储在响应对象中的错误捕获功能
3.  钩子应该不知道传递给它的是什么类型的 API 调用
4.  API 调用库应该独立于钩子，易于维护，并且是为项目定制的

此外，钩子的用户流应该简单。开发人员应该能够导入钩子，导入必要的 API 调用函数，并像使用 useState 钩子一样使用它。

**那么它是如何工作的呢？**

useApi 自定义挂钩由两个文件组成:

1.  `hooks/useApi.js`:通过 API 调用接收匿名回调函数并返回响应对象和 setter 函数的钩子
2.  `api/index.js`:导出的 API 调用函数库，可以传递给 useApi 挂钩的匿名回调函数

假设您正在构建一个显示志愿者姓名列表的组件。学生将登陆该页面，该组件将呈现所有可用志愿者的列表，以帮助他们导航他们的学生体验。

为了让组件获得志愿者列表，它必须用志愿者数据向 API 发出 get 请求。为了在这种情况下利用 useApi 挂钩，我们将在 Api 调用库中编写一个 API 调用函数并将其导出。让我们称这个函数为 fetchResource。该函数应该将关键字“志愿者”作为参数，因为这是要获取的资源。然后，我们将 useApi 挂钩和 fetchResource 导入到组件中。我们将设置挂钩并利用 useEffect 在组件挂载时设置一个 volunteersResponse 变量。既然我们已经有了 volunteersResponse 状态，我们可以根据需要在组件中呈现数据。

```
import { **useEffect** } from "react";
import { **useApi** } from '../../utils/hooks/useApi';
import { **fetchResource** } from '../../utils/api';function **StudentLandingPage**() {
  const [volunteersResponse, setVolunteersResponse] = **useApi**(() => **fetchResource**("volunteers"));**useEffect**(() => {
  setVolunteersResponse();
}, []);return (
  <div className="container">
    <p>volunteersResponse is set. Format it and display it!</p>
  </div>
  )
}
```

**引擎盖下**

首先，useApi 挂钩利用 React 的 useState 来管理 Api 响应的切片集。默认对象包含一个设置为 null 的数据属性，响应最终将设置在该属性中。它还包括请求状态(`isFetching`)、错误处理(`error`)和成功响应(`isSuccess`)的属性。

```
import { **useState** } from 'react';export function **useApi**(**apiFunction**) {
  const [**response**, **setResponse**] = **useState**({
    data: null,
    isFetching: false,
    error: null,
    isSuccess: false
  });

  ...
```

接下来，钩子定义了一个 fetchMethod，当调用它时，将触发 API 调用及其相关的状态变化。具体来说，它将更改请求状态，以便系统知道请求正在进行中，并发出 API 调用。它将等待来自 API 的响应，并相应地设置响应对象的状态。

```
 ... const **fetchMethod** = () => {
    **setResponse**({
      data: null,
      isFetching: true,
      error: null,
      isSuccess: false}); **apiFunction**()
      .then(res => {
        **setResponse**({
          ...response,
          data: res,
          isFetching: false,
          isSuccess: true
        })
      })
      .catch(err => {
        **setResponse**({
          ...response,
          isFetching: false,
          error: err.message
        })
      })
  }; ...
```

最后，钩子返回响应对象和 fetchMethod 函数。通过这样做，我们公开了希望钩子操作的状态片段，也公开了它的 setter 方法——在本例中是 fetchMethod 函数。因此，对于钩子的用户来说，我们镜像了 React 开发人员非常熟悉的 useState 模式。

```
 ... return [**response**, **fetchMethod**];
}
```

请注意，开发人员传入了一个匿名函数，该函数返回 API 调用函数。这是为了当钩子被设置时，API 调用函数不会被调用。而是在执行 fetchMethod 时执行 API 调用函数。而且由于设置钩子时 fetchMethod 作为 setter 返回，所以当开发人员准备好从 API 设置他们需要的响应时，就会调用 API 调用函数。

**结论**

这个实现为我们最近在 Lambda 学校部署的项目节省了时间。前端团队能够专注于构建优秀的组件，而不用担心充斥着嵌入式 API 调用和重新渲染的臃肿代码。此外，我们能够构建一个更简洁的代码库，最终更易于维护和移植。