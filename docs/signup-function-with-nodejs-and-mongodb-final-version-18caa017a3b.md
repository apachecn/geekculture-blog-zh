# 用 NodeJs 和 MongoDB 注册函数(最终版本)

> 原文：<https://medium.com/geekculture/signup-function-with-nodejs-and-mongodb-final-version-18caa017a3b?source=collection_archive---------34----------------------->

![](img/df4b50b5de122810d943d2eb7791de77.png)

我写了这个函数和其他函数，但是在我写的时候这个项目还没有完成。所以，我完成了这个项目，并想解释如何去做。开始吧！

首先，我们需要在 MongoDB 中创建一个数据库。为此，我们将使用模式。在这个项目中，我有三个数据库。我们将从用户模式开始。

为了创建一个模式，我们需要 mongoose。然后我们就写这段代码行。

```
const UserSchema = **new** *mongoose*.Schema({})
```

在数据库中，我们有名称，电子邮件，密码，点，randomCode，登录，forgotCode，id，imageUrl 和 refreshToken。随机代码用于激活帐户，forgotCode 用于重置密码，refreshToken 用于进入帐户等其他页面。姓名，电子邮件和密码是必需的，我使用以下正则表达式检查电子邮件格式。

```
/^(([^<>()[\]\\.,;:\s@"]+(\.[^<>()[\]\\.,;:\s@"]+)*)|(".+"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-Z\-0-9]+\.)+[a-zA-Z]{2,}))$/
```

在模式的结尾，我们将导出这样的模型:

```
*module*.*exports* = *mongoose*.model('User', UserSchema);
```

我们将在其他页面中使用“用户”。我将创建一个名为 controllers 的文件夹，并在文件夹中创建一个名为 auth.js 的文件。Nodemailer 用于向地址发送电子邮件。我们将为 nodemailer 创建一个名为 transporter 的变量。然后，我们将创建一个电子邮件地址，并将其用于 nodemailer。

```
var transporter = nodemailer.createTransport({service: 'hotmail',auth: {user: process.env.NODEMAILER_USER,  // in .env filepass: process.env.NODEMAILER_PASS // in .env file}});
```

现在我们将创建异步寄存器函数。在这种情况下，我们将让一个令牌和 6 位数的随机数相等。

```
let token = Math.floor(Math.random() * 999999);
```

然后让 refreshToken 和定义 const name，email 和 password 等于 req.body。req.body 是请求的主体。之后，我们将创建一个名为 data 的变量。

在数据中，我们有 from、to、subject 和 html 部分。在 html 中，我发送我们创建的令牌。数据是这样的:

```
var data = {from: process.env.NODEMAILER_USER,to: email,subject: '',html: `<h1>Hello ${name} ! Please copy token that sent.</h1><h1>${token}</h1>};
```

然后，我们将控制用户是否存在给定的电子邮件，如果存在，我们将写:

```
const mailSucces = transporter.sendMail(data, async function (*error*, *info*) {}
```

如果有错误，我们将返回一个错误。否则，我们将像这样创建一个用户:

```
const user = await User.create({name,email,password,login: false,point: 0,randomCode: token,id: count,refreshToken});
```

我们将定义 rtoken 并在用户模型中创建一个 jwt 令牌。代码在这里:

```
UserSchema.methods.getSignedJwtToken = function () {return jwt.sign({ id: *this*._id }, process.env.JWT_SECRET, {expiresIn: "30d"})};
```

有效期是 30 天，也就是 token 的销毁时间。

我们将用户的 refreshToken 设置为 rtoken，然后保存用户:

```
const rtoken = user.getSignedJwtToken();user.refreshToken = rtoken;user.save()
```

最后，我们将返回一条消息并进行检查。

```
return *res*.json({message: "Email Gönderildi",register: true,rtoken});
```

就这样，我们将继续激活帐户。感谢阅读！