# Hook 路由器简介:React 的新路由库

> 原文：<https://medium.com/geekculture/hook-router-new-rouging-for-react-js-8f53f540bb4f?source=collection_archive---------25----------------------->

![](img/d22eade14e2ea08662057a3cc2c41af4.png)

hookrouter.js

# 介绍

React.js 不支持开箱即用。React Router 是一个非常流行的库，它与 React.js 一起使用来实现路由。

Hook Router 是 react.js 中启用路由的新库，它是围绕 react 钩子构建的。它非常简单小巧。它附带了一个简单而整洁的文档。一个新的 react 开发人员很容易用它弄脏手。

在本课中，我将使用`hookrouter.js`深入研究 react 路由。我们将创建一些带有导航链接的路由。

# 概观

举个例子比较好理解。为了容易理解，我尽量使它简单。我们演示了一个 react 应用程序，它由四个路径组成: **home** 、 **about** 、 **users** 和 **user-detail** 。我们还在 ***用户*** 和 ***用户详细信息*** 页面中演示了**动态路由**。

让我们使用流行的命令行`npx`创建一个 react.js 应用程序，并安装`hookrouter`。

```
npx create-react-app routing-demo
cd routing-demo
npm i -S hookrouter
```

我们在 src 文件夹中创建以下组件:

1.  Menu:保存菜单列表并使用
2.  Home:是主页。
3.  关于:是关于页面。
4.  Users:显示从 API 读取的用户列表。
5.  UserDetail:显示单个用户的详细信息，从 API 中读取。
6.  NotFound:是当 url 不匹配任何路由时应用程序转到的页面。

```
/components/Menu.js
/pages/Home.js
/pages/About.js
/pages/NotFound.js
/pages/Users.js
/pages/UserDetail.js
```

我们删除 src 文件夹中的以下文件:

```
logo.svg
App.test.js
App.css
```

> 我们还需要删除 index.js 中 react 的严格模式，否则点击`<A href="/about" />`链接不会重定向到 about 页面。

# 按指定路线发送

我们将路由定义为一个普通的 javascript 对象。每个属性名是路由 URL，值是 react 组件(在我们的例子中是 routes: **home** 和 **about** )。

```
const routes = {
  **"/": () => <Home />,**
  "/about": () => <About />,
};
```

在 App 组件内部，我们将路由从`reactrouter`库传递给`useRouter()` **钩子**，以将路由注册到它们对应的组件。

我们从 App 组件返回一个`match` ed route 否则`NotFound`页面如下:`***{ match || <NotFound /> }***` ***。***

```
***import { useRoutes } from "hookrouter";*****const routes = {...};**function App() { ***const match = useRoutes(routes);*** return (
    <div className="App">
      <Menu />
      <div className="container">***{match || <NotFound />}***</div>
    </div>
  );}
```

## 链接

链接用于在页面之间导航。在菜单组件中，我们定义了链接。使用`hookrouter`库中的`A`组件定义链接。

```
***import { A } from "hookrouter";***const Menu = () => {
  return (
    <nav className="App-Header"> ***<A href="/">Home</A>
      <A href="/about">About</A>*** </nav>
  );
};
```

# 动态路线

路由最重要的特性之一是处理动态路由。有时我们需要在 URL 中有一个动态值，比如一个 ID，并基于它从服务器获取数据或显示一些信息。

我们通过例子来演示这个特性。我们显示了从后端服务器获取的用户列表。我们还为每个用户添加了一个按钮来显示用户的详细信息。在链接中，我们应该有一个动态值，如 ID，以知道显示哪个用户。

> 在路由中，通过在 URL 部分的开头添加冒号:来定义参数。

1.  增加两条额外路线:`/users`和`**/users/:id/detail**` **。**
2.  增加两个组件:`Users`和`UserDetail`。
3.  在`UserDetail`路线中将`id`作为参数传递。

```
const routes = {
  .....
  "/users": () => <Users />,
  **"/users/:id/detail": ({ id }) => <UserDetail id={id} />,**
};
```

## 用户页面

用户页面显示从 [{JSON}占位符](https://jsonplaceholder.typicode.com/users)中读取的用户列表。每个用户都有一个指向用户详细信息的链接，URL 包含用户的 ID。

```
import { A } from "hookrouter";const Users = () => {
  const [users, setUsers] = useState([]); useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users")
    .then((res) => res.json())
    .then((result) => setUsers(result));
  }, []); return (
    <table id="users">
      <thead>
        <tr>
          <th>#</th>
          <th>Name</th>
          <th></th>
        </tr>
      </thead>
      <tbody>
        {users.map((user) => (
          <tr key={user.id}>
            <td>{user.id}</td>
            <td>{user.name}</td>
            <td>
              ***<A href={`/users/${user.id}/detail`}>Details</A>***
            </td>
          </tr>
        ))}
      </tbody>
    </table>
  );
};
```

## 用户详细信息页面

用户的 ID 被传递到用户详细信息页面。ID 用于从后端服务器获取用户详细信息: [{JSON}占位符](https://jsonplaceholder.typicode.com/users/1)。

```
const UserDetail = (***{ id }***) => {
  const [user, setUser] = useState({}); useEffect(() => {
    fetch(`***https://jsonplaceholder.typicode.com/users/${id}***`)
    .then((res) => res.json())
    .then((result) => setUser(result))
  }, [id]); return (
    <div>
      <h3>User Detail:</h3>
      <p>ID: {user.id}</p>
      <p>Name: {user.name}</p>
      <p>Username: {user.username}</p>
      <p>Phone: {user.phone}</p>
      <p>Email: {user.email}</p>
      <p>Company: {user.company?.name}</p>
    </div>
  );
};
```

# 结论

使用`hookrouter.`在 react.js 应用程序中启用路由非常容易，我试图介绍路由的基础知识，但是还有更多库的特性没有介绍，比如`redirecting`和`programmatic navigation.`

完整的源代码，请访问这个 [git 库](https://github.com/bewarusman/hookrouter-demo)。