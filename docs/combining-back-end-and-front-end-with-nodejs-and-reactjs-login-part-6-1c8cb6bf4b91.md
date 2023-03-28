# 将后端和前端与 NodeJs 和 ReactJs-Login 结合起来(第 6 部分)

> 原文：<https://medium.com/geekculture/combining-back-end-and-front-end-with-nodejs-and-reactjs-login-part-6-1c8cb6bf4b91?source=collection_archive---------15----------------------->

![](img/b91002dd32bd4216cfc2c19f6bff7626.png)

在本文中，我将继续登录部分。对于以前的文章，你可以去个人资料。我们开始吧。

像以前的文章一样，我会给你一个登录表单。我不写它的代码，CSS 在这个页面上不存在。函数的返回部分如下:

```
<div *className*="wrapper1">
<div *className*="form-wrapper">
<h1>{t('Sign.signin')}</h1><form ><div *className*="email"><label *htmlFor*="email"></label><input*type*="text"*className*=""*placeholder*="E-mail"*type*="email"*name*="email"/></div><div *className*="password"><label *htmlFor*="password"></label><input*type*="text"*className*=""*placeholder*="Password"*type*="password"*name*="password"
/>
</div><button *className*="buttonsign" *value* = "Submit">
Login<h5>{t('Sign.signin')}</h5>
</button><*Link* *className*="linksignin" *to*="/signup" >{t('Sign.accountUp')}</*Link*></form></div></div>
```

对于函数，我们将编写:

```
export default function SignIn(){
}
```

现在，我们必须设置名为 email 和 password 的变量。为此，我们将使用 useState()。那我们就写:

```
const history = useHistory();
```

在这个函数中，我们也需要一个小函数。这将是一个名为 submit 的异步函数。在 submit 中，我们将让一个名为 item 的变量等于{ email，password }。然后让一个变量也，并给出名称结果。在此之后，我们必须获取数据:

```
let result = await fetch("http://localhost:5000/login",{method: "POST",body:JSON.stringify(item),headers: {"Content-Type":"application/json","Accept":"application/json"}}).then(history.push("/") )
```

Url 是关于路线的，所以如果你遵循这个教程，链接看起来就像这样。方法是 POST，主体是 item，我们有 headers。之后，我们将使用 history . push(“/”)。这将把我们带到主页。可能以后会改吧。

然后

```
result = await result.json()
console.log(“result”, result)
```

结果将显示我们之前编写的后端响应。

现在，我们将转到第一个 div，并将其添加为:

```
*onChange* =  {(*e*) => setEmail(*e*.target.value)}
```

对于第二个 div，只需将 setEmail 更改为 setPassword。最后，转到按钮并添加 onClick{submit}。

有了这个，文章就完成了！现在，和上一篇文章一样，我们将从后端开始

```
npm run dev
```

和前端

```
npm start
```

如果我们将确认写为 true，它将返回成功:true 和一个令牌。如果确认为假，成功将为假，错误:无效凭证。

在下一篇文章中，我将解释如何组合注销部分。感谢阅读！