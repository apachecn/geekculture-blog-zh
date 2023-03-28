# 在 Next.js 上，React Query，并学习爱上 async/await

> 原文：<https://medium.com/geekculture/on-next-js-react-query-and-learning-to-love-async-await-c95755efcb95?source=collection_archive---------9----------------------->

![](img/2e4285e2a3203f3501beb1a497e82db2.png)

**React Query** 是 Tanner Linsley 为 React/Next 系列框架编写的一个巧妙的钩子和工具包。我正在熟悉它，因为我很快就要为几个严重依赖于**异步数据的 web 应用做出贡献。**到处飞着`Promise` s 和`fetch` es 管理状态让我眩晕发作……不过 Next.js 和 React Query 稍微缓解了恶心——有个学习曲线。

假设我有一个待办事项应用程序。Next 中的前端有一个任务列表和一个控件表单；当用户添加任务时，前端应该更新 DOM *和*用`fetch( "...", { method: "POST" } )`更新后端。表单看起来有点像这样:

```
import { useState } from 'react';import homeStyles from '../styles/Home.module.css';const AddToDoForm = () => { const [ toDoFormState, setToDoFormState ] = useState( "" ); return <form
    className={ homeStyles.form }
    onSubmit={ *...a fetch/post/state change of some kind...* }
  >
    <input
      value={ toDoFormState }
      onChange={ changeEvent => setToDoFormState( changeEvent.target.value ) }
    />
    <input type="submit" value="Do it!" />
  </form>;};export default AddToDoForm;
```

如果没有 React Query，我可能会决定在`App`组件中创建一个全局状态，然后将 getters 和 setters 作为`props`传递，以保持全局状态随着 I `fetch`更新。如果我觉得冷，我甚至会设置一个 Redux `store`。但是……这种方法有异步数据的味道。为什么要浪费时间和代码来确保数据库和国家总是以同样的速度跳舞呢？

有了 React 查询，我可以整天都不用担心全局状态。我只是给我的应用程序一个 URL、一个方法和一些数据，指出更新应该在哪里/什么时候发生……然后 React Query 做剩下的事情。为了实现这一点，在安装了`npm i react-query`或`yarn add react-query`之后，我返回到应用程序的索引页面，将整个组件包装在一个`QueryClientProvider`中:

```
**import { QueryClient, QueryClientProvider } from 'react-query';**import AddToDoForm from '../components/AddToDoForm';...**const queryClient = new QueryClient();**const ToDoCards = () => { *...component with a div for each task...* }export default function Home() { return **<QueryClientProvider client={ queryClient }>**
    *...* <AddToDoForm />
    <ToDoCards />
  **</QueryClientProvider>**;}
```

就像这样，现在时间的英雄有了一个`queryClient`，他获得了神奇的`useQuery`钩子，给他管理全局状态的能力，因为他`fetch` es:

```
import { QueryClient, QueryClientProvider, **useQuery** } from 'react-query';...const ToDoCards = () => {
 **const { isLoading, data } = useQuery( "todos", async () => {
    const response = await fetch( `...` );
    return response.json();
  } );**
  return <div className={ homeStyles.grid }>
    { **isLoading ? "Loading..."** : **data.map**( todo => <ToDoCard
      key={ todo.id }
      todo={ todo }
      allTodos={ data }
    /> ) }
  </div>;
}export default function Home() { return *...*; }
```

`useQuery`接受两个参数:一个**键，**用于在将来引用该资源，一个回调实际上`fetch` es 和`return` s some `json()`。`useQuery`也返回其他变量去分解，像`status`和`error`消息；在这种情况下，我使用了`isLoading`布尔值来显示一个很好的加载消息，在这个消息中，用户等待响应时卡仍然存在。

用于发出`POST`和`PATCH`请求的 React 查询钩子是`useMutation()`。让我们将它放回到`AddToDoForm`组件中:

```
**...****import { useMutation, useQueryClient } from 'react-query';****const handleSubmission = async ( { toDoFormState } ) => {
  const response = await fetch( `http://localhost:${ 3001 }/todos`, {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify( { *...new todo from ToDoFormState...* } )
  } );
  return response.json();
};**const AddToDoForm = () => { **const queryClient = new QueryClient();** const [ toDoFormState, setToDoFormState ] = useState( "" ); **const { mutate } = useMutation( handleSubmission, {
    onSuccess: async newToDo => {
      setToDoFormState( "" );
      await queryClient.cancelQueries( "todos" );
      const previousValue = queryClient.getQueryData( "todos" );
      queryClient.setQueryData( "todos", previousQueryData => [ ...previousQueryData, newToDo ] );
      return previousValue;
    },
  } );** return <form
    className={ homeStyles.form }
    **onSubmit={ async submissionEvent => {
      submissionEvent.preventDefault();
      try { mutate( { toDoFormState } ); }
      catch ( error ) { console.log( error ); }
    } }**
  >
      <input
        value={ toDoFormState }
        onChange={ changeEvent => setToDoFormState( changeEvent.target.value ) }
      />
      <input type="submit" value="Do it!" />
    </form>;};export default AddToDoForm;
```

注意:

*   就像以前一样，我正在同步返回`json()``fetch`ed`async`——但是请注意，在我的`handleSubmission`函数中，我正在从参数中的受控形式中去结构化`toDoFormState`。
*   `mutate`函数是从`useMutation`中分解出来的，它有两个参数:我刚刚定义的用于生成`fetch`的`handleSubmission`回调，以及一个带有三个键中的一个或多个的对象:`onError`、`onSuccess`和/或`onSettled`(错误或成功)，每个键都指向一个函数，其中包含一些用于我们的`queryClient`的指令，用于响应每种情况。为了简洁起见，这里我只定义了一个`onSuccess`方法，其中我 1)清除表单状态，2)为我们的`“todos”`键定义`...previousQueryData`键，新的数据库条目附加在末尾，3)返回以前的查询数据。
*   最后，我可以调用`mutate( {} )`——当然，确保用`try {} catch () {}`包装它，以防`fetch`出错。

React Query 并不是从日志上掉下来的，但是在一个持续不断的应用程序中，我认为这是非常值得的。没有像`todosToDisplay`或`setTodosToDisplay`这样的状态道具……不需要担心`dispatch`或`store`或`context`……还有许多其他的缓存和滚动挂钩来提高应用程序的性能。

上面的代码放在这里[这里](https://github.com/josh-frank/youcandoit-front)，大量的文档/示例放在这里[这里](https://react-query.tanstack.com/overview)。