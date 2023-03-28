# 将后端和前端与 NodeJs 和 ReactJs 结合起来——重置密码(第 8 部分)

> 原文：<https://medium.com/geekculture/combining-back-end-and-front-end-with-nodejs-and-reactjs-reset-password-part-8-a53398d11747?source=collection_archive---------4----------------------->

![](img/dc8a657834cf3e1feb96769997a22d33.png)

在这篇文章中，我将解释如何编码忘记/重置密码，如何发送电子邮件，以及如何将这些文件与前端结合起来。如果你没看过我之前的文章，可以去我的简介。让我们从忘记密码开始。

首先，我们需要 mailgun。我们去 mailgun 网站并添加一个帐户。注册申请是免费的，所以你可以免费使用。之后，我们将在终端中编写 npm install mailgun-js 并安装包。我们将转到 controllers/auth.js 文件，将 mailgun-js 作为 mailgun 导入，并删除其中的 register 方法，我们将重新编写它。

现在，我们要去

```
[https://app.mailgun.com/app/dashboard](https://app.mailgun.com/app/dashboard)
```

还有发送/总览，复制链接没有？，将其粘贴到域变量中。

然后我们将创建另一个变量:

```
*const mg = mailgun({apiKey: process.env.MAILGUN_APIKEY, domain: DOMAIN});*
```

我们需要 MAILGUN_APIKEY，所以我们将去。env 文件并创建 MAILGUN_APIKEY 变量。为此，在 mailgun 站点上，我们将单击 API 部分并选择 Node.Js。我们将复制 API 密钥并将其粘贴到 MAILGUN_APIKEY 中。

之后，我们将把 forgotCode 作为一个数字添加到用户数据库中。那么我们可以从 forgotPassword 函数开始。

在这个函数中，我们将向我们的用户发送一封电子邮件。在电子邮件中，我们为支票用户提供了一个 6 位数的代码。我将把它创建为一个全局变量，这样我们就可以在这个页面的任何地方访问它:

```
let forgotToken = Math.floor(Math.random()*999999);
```

你可以用 jwt 来检查用户，我解释过了。该函数将是异步函数。

我们将在请求中收到一个电子邮件地址。然后，等于 req.forgotCode 到 forgotToken。req.forgotCode 在架构中。它可以被设置，但如果我们不在 activateAccount 部分给它，它将是不可见的。

在电子邮件中，我们将发送数据。数据将如下所示:

```
const data = {from: ' *',to: email,subject: '* ',html:`<h2>Hello ! Please copy token that sent.</h2><p>${forgotToken}</p>`};
```

你可以写任何关于电子邮件的东西。只是从'到'部分将电子邮件和 HTML 部分必须${forgotToken}。

然后，我们使用 mg 变量来发送数据。

```
mg.messages().send(data, function (*error*, *body*) {if(*error*){return *res*.json({message: *error*.message})
}return *res*.json({message : "email has been send"})
});
```

如果发送函数返回成功，邮件将被发送。然后我们会重设密码。为此，我们将创建一个 resetPassword 函数。

在 resetPassword 中，我们请求三个变量:forgotCode、email 和 Password。该电子邮件将用于在数据库中搜索。

如果 forgotCode 存在并且等于 forgotToken，那么我们将搜索数据库并用新密码重置密码。代码在这里:

```
const {forgotCode, email, password } = *req*.body;if(forgotCode){if(forgotCode == forgotToken){const user = await User.findOne({email: email})user.password = password;user.save();*res*.status(200).json({success: true})}else{*res*.status(400).json({success: false,message: "Wrong code!"})
}
}
```

现在，我们将使用这两个。为此，我们将转到 routes/auth.js 文件并添加以下代码:

```
router.post('/forgotPassword', forgotPassword);router.put('/resetPassword', resetPassword);
```

ForgotPassword 是 post 请求，resetPassword 是 put 请求。

后端部分结束了，我们来创建两个页面:ForgotPassword.js 和 ResetPassword.js，前端部分在这里:

```
<div *className*="wrapper1"><div *className*="form-wrapper"><h1>Forgot Password</h1><form ><div *className*="email"><label *htmlFor*="email"></label><input*type*="text"*className*=""*placeholder*="E-mail"*type*="email"*name*="email"
/></div><button *className* = "buttonsign"*value* = "Confirm"><h5>Confirm </h5></button></form></div></div>
```

我们会把它写到函数的返回部分。然后，我们将使用 useState(" ")来设置电子邮件。在 function 中，我们也将创建一个新的异步函数，名为 forgotPassword。

在 forgotPassword 中，我们将一个名为 item 和 equals 的变量赋给 email。

```
let result = await fetch("http://localhost:5000/forgotPassword",{method: "POST",body:JSON.stringify(item),headers: {"Content-Type":"application/json","Accept":"application/json"}}).then(history.push("/resetPassword")) // redirect to pageresult = await result.json()console.log("result", result)
```

现在，我们将在如下按钮中使用该函数:

```
*onClick*={forgotPassword}
```

在 div 中，我们将添加以下代码:

```
*onChange* =  {(*e*) => setEmail(*e*.target.value)}
```

现在我们将转到 resetPassword 页面。在这个页面上，我们有三个 div 和一个按钮。电子邮件、resetToken 和 newPassword 有三个部分。我们将用 useState(" ")来设置它们。我们将像这样创建一个名为 resetPassword 的异步函数:

```
const resetPassword = async (*e*) => {let item = { email, newPassword, resetToken }console.log(item)let result = await fetch("http://localhost:5000/resetPassword",{method: "PUT",body:JSON.stringify(item)}).then(history.push("/") )result = await result.json()console.log("result", result)
```

然后，在 div 中使用 set 函数，如下所示:

```
*onChange* =  {(*e*) => setEmail(*e*.target.value)}
```

并像这样使用 resetPassword 函数:

```
*onClick*={resetPassword}
```

在这些之后，如果你去 localhost:3000/forgotPassword，你可以看到输入和一个按钮。如果你写了一封电子邮件，存在于数据库中，一个随机代码将被发送，你将重定向到重置密码页面。在这个页面上，您可以看到三个输入和一个按钮。如果您写入真值，密码将被更改。

就是这样！本文已完成，感谢阅读。