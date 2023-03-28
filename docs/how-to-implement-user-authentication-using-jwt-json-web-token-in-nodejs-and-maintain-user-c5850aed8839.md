# 如何在 NodeJS 中使用 JWT (JSON Web Token)实现用户认证，在 React 中使用会话存储维护用户会话？

> 原文：<https://medium.com/geekculture/how-to-implement-user-authentication-using-jwt-json-web-token-in-nodejs-and-maintain-user-c5850aed8839?source=collection_archive---------4----------------------->

![](img/63beed0433c004f4fdef57a9341fe557.png)

当创建一个核心功能是面向用户的 web 应用程序时，这意味着用户必须注册到系统中才能使用该应用程序，那么用户身份验证和用户会话就起着关键作用，如果这两件事没有正确实现，那么整个系统就会一塌糊涂。如果不遵循这些标准，您的系统就容易受到安全威胁。因此，在应用程序中遵循这些用户认证和会话处理标准始终是一个好习惯，即使你是在大学里为班级级项目这样做。在这里，我将向您展示如何使用 JWT 和会话存储来完成这些工作。

**JWT(JSON Web Token)** 是一个独立的对象，它允许在双方(客户机-服务器)之间安全地传输 JSON 对象形式的数据。JWT 的三个主要用途是身份验证、授权和数据交换。

**sessionStorage** 允许在 web 浏览器中存储键值对。还有另一个存储对象有助于以相同的方式存储值，它是 **localStorage** ，但不同之处在于，sessionStorage 只保存一个会话的数据，这意味着当浏览器选项卡关闭时，会话值将过期，但 localStorage 保存数据，即使您关闭浏览器，这意味着您可以在下次不登录的情况下开始使用网站，但您仍然可以使用清除存储数据。**清除**方法，你可以通过实现注销功能来完成。因此，无论如何，这取决于您根据您的业务逻辑在两者之间做出决定。

在进入它如何工作之前，首先，我们必须下载所需的模块依赖。

**npm 安装 jsonwebtoken**

## **认证(登录)**

```
handleSubmit=(event)=> {

    event.preventDefault();

    const user = {
        username: this.state.username,
        password: this.state.password
    }

    ***axios***.post('http://localhost:5000/user/login',user)
        .then(res =>{

          ***sessionStorage***.setItem("token",res.data.accessToken);
                ***window***.location="/home this.setState({
                username: '',
                password: ''
            })})catch(e=>{
    alert(e.response.data.error);
    this.setState({
        username: '',
        password: ''
    })
})}
```

这里这个 **handleSubmit** 方法被分配给前端登录表单的 **onSubmit** 方法，Axios 在这里被用来发送 HTTP 请求，在这个例子中我使用了 **pos** t 方法。该请求将被发送到服务器(ExpressJS ),其主体中附加了包含用户名和密码的对象。

这里我将使用 ExpressJs 服务器，所以我假设您知道如何实现它。在执行 HTTP 方法处理的 JS 文件(路由文件)中加载 jwt 依赖项。

```
const ***jwt*** =require('jsonwebtoken'); ***router***.post("/login",async(req,res)=>{

    let user = req.body;

    if(user.username==="user" && user.password==="pwd"){

        const accessToken=***jwt***.sign({username:user.password},"secret");
        res.status(201).send(accessToken)

    }else {
         res.status(502).json({error:"Wrong username or Password"})
         }    
})
```

现在，一旦后端接收到请求并且给定的凭证有效，JWT 将使用对上下文有意义的数据(例如:用户名)和任何随机值(可以是任何字符串值或数字)生成一个唯一的令牌，我已经给定了一个字符串值“secret”。生成令牌后，它将作为响应数据传递给前端。

**授权**

现在，一旦用户登录到系统，他/她就可以从那里开始执行任务，但是用户的会话必须是活动的，以允许他们自始至终使用系统。因为即使你有一个认证机制，但你没有任何授权的概念，这仍然会使你的系统容易受到潜在的安全威胁，特别是用户的隐私也受到威胁。

```
***sessionStorage***.setItem("token",res.data.accessToken);
```

这就是我们要使用 sessionStorage 对象来存储会话值的地方。在这里，我存储了通过 HTTP 响应传递的 jwt 令牌。如果存在系统需要存储执行特定动作的用户的详细信息的任何功能，这可能是有帮助的。这就是我们接下来要看到的！！

**数据交换**

每当用户通过 HTTP 请求执行某些操作，如数据处理(get、delete、create、update ),那么保存在会话存储中的这个 jwt 令牌可以与 HTTP 请求一起发送，以验证特定操作是由授权用户执行的。

```
***axios***.post('http://localhost:5000/user/add,object,{
    headers:{
        Authorization:***sessionStorage***.getItem("token")
    }
} )
```

在这里，我发送一个 post 请求，其主体附加了一个对象，此外，我还通过添加标题名“Authorization”并给出登录后保存在 sessionStorage 中的 jwt 令牌的值来传递标题信息。

```
const {auth}=require('../middleware/auth')//importing auth function  from another directory***router***.post("/add",auth,(req,res)=> {

  //action to be performed

})
```

在这里，如果您查看这个 post 方法处理，并将它与之前用于登录的 **post** 方法进行比较，您可以看到这里出现了一个额外的参数**‘auth’**。这是检查用户有效性的地方。

```
const ***jwt*** =require('jsonwebtoken');

function auth(req,res,next){

    const authHeader=req.header('authorization');

    //check token
    if(authHeader==null){
        return res.status(401).json({error:"Access-denied"});
    }

    //check validity
    try{
        const verified=***jwt***.verify(authHeader,"secret");
        req.id={username:verified.username}; //if verified the token will be decoded and the username of the user will be extracted and passed.
        next();

    }catch (e){
        res.status(401).json({error:"Invalid-token"});
    }

}

module.exports={auth}
```

这里是检索头值和验证 jwt 令牌的地方，方法是提供我们在头中传递的令牌值和在登录处理过程中为生成 jwt 令牌而给出的随机值。在这种情况下，如果您从会话中提供的令牌是用值“secret”生成的，则它将被成功验证，并且数据(用户名)和“secret”将可以作为对象传递(已验证)。现在使用这个 **req.id** ，它现在保存了执行该操作的用户的用户名，如果需要记录该值，您可以将该值传递给数据库。

这个 auth 函数是在一个单独的 js 文件中实现的，所以您不需要在您创建的所有路由器文件中重写相同的函数。

**注销**

以上 3 个部分向您展示了从用户认证到授权再到授权数据交换的流程。通常情况下，一旦您使用完网站，您可能希望注销或不注销。特别是如果你已经从公共设备或其他人的设备登录到一个网站，那么强烈建议注销。就像我前面提到的，如果会话存储在会话存储中，那么一旦关闭浏览器，它将自动清除存储，但如果它是使用本地存储存储的，那么它将永远保留。不管怎样，有一个通用的标准，如果你的应用程序有一个登录功能，那么也必须有一个注销功能。

```
doLogout=()=>{

    ***sessionStorage***.clear();
    ***window***.location="/login"

}
```

这里实现注销功能非常容易。您只需要创建一个按钮，将 onClick 分配给这里提到的注销功能。

**额外备注**

如果有任何情况，比如在前端显示登录用户的用户名或电子邮件或任何标识，您可以使用该值生成 jwt 令牌，并像我们在这里所做的那样将其传递给前端，然后解码该令牌以检索用户名。下面是如何做的，

npm i jwt-decode //下载此依赖项

从“jwt-decode”导入解码；//导入

decode(sessionStorage.token)。用户名

//将它赋给一个状态变量或你想显示它的地方。

就这样…谢谢…玩得开心…