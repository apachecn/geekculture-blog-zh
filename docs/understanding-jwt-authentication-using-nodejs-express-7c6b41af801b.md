# 使用 NodeJS & Express 了解 JWT 认证

> 原文：<https://medium.com/geekculture/understanding-jwt-authentication-using-nodejs-express-7c6b41af801b?source=collection_archive---------16----------------------->

![](img/b257ec18b9d793168315ef019d2a879d.png)

本文主要面向那些希望理解 JWT 术语以及如何在应用程序中生成和使用 JWT 的读者。

JWT 的基本原理在任何开发平台上都是一样的，通过本文获得的知识可以应用于所有开发平台。在本文中，我将使用 NodeJs 来演示该功能。

我假设你知道常见的节点和表达概念。如果没有，我强烈建议在继续阅读本文之前查看一下。

本文中演示的示例应用程序的链接可以在文章末尾找到。

# 什么是 JWT？

> JSON Web 令牌是一种开放的行业标准 [RFC 7519](https://tools.ietf.org/html/rfc7519) 方法，用于在双方之间安全地表示声明。
> 
> *jwt.io*

JWT 是发展世界中最常用的术语之一。它们主要用于交换信息，或者作为 API 生态系统中的一种认证/授权模式。

# JWT 的基本结构

JWT 通常看起来像一个随机的字符串，但它肯定有一个结构。

```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```

在上面的例子中，正如你所看到的，令牌有三个不同的组成部分，由(。)句号。每个组件都由不同的信息集组成，这些信息是字符串有意义所必需的。

![](img/1f342fc9d71ac790140df895bdb7ead5.png)

## 页眉

令牌的头是一个 json 对象，包含重要的信息，如用于创建令牌的算法类型、类型、与令牌相关的密钥 id，以防您计划使用公钥/私钥等。下面是根据上面共享的 RFC 文档允许的几个可接受值的列表。

*   **算法**
*   **类型**
*   **cty**

除此之外，您还可以使用诸如“kid”等私有声明来提供有关令牌的信息。

## 身体

这是令牌的一部分，所有公共和私人声明都存在于此。您可以参考 RFC 规范，了解更多关于标记为公开的声明的信息。几种常用的说法如下

*   **iss** — **令牌的发行者**
*   **aud** — **此代币面向的受众**
*   **子** — **主题**索赔。指定令牌的主体。
*   **到期** — **到期**索赔。对于令牌，在此之后令牌将被视为无效。
*   **nbf** — **不在**索赔之前。这指定了令牌将被视为有效的时间。这通常是可选的，但在某些情况下非常有用，在这些情况下，令牌将在一段时间后被激活。
*   **iat** — **在**发出索赔。颁发者颁发令牌以供使用的时间。
*   **jti** — **JWT Id** 认领。一般来说，这是一个唯一的标识符，有助于您使用令牌。

## 签名

令牌的这一部分带有由发行者生成的签名，这有助于发行方验证令牌的真实性。

```
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload), 
your-256-bit-secret
)
```

上面的片段说明了签名是由什么组成的。如果对令牌中包含的信息进行了任何修改或篡改，签名将不会匹配，因此将告诉发行方存在需要解决的安全问题，或者有人试图访问他们无权访问的信息。

# JWT 的类型

在我们深入研究生成和合并令牌的细节之前，我们需要知道存在什么类型的令牌。

令牌的基本结构保持不变。它们的区别是基于它们在体内所携带的要求。根据这一主张，他们被分为

## IDToken

这些类型的令牌在有效负载中携带用户信息。一般来说，伴随着公开声明的还有识别用户身份的信息。

最常见的例子是使用谷歌或脸书单点登录生成的令牌。

您应该非常小心地使用这些类型的令牌，因为全球各地都实施了法规遵从性。

```
//Sample body of IDtoken
{
  "iss": "http://abc.com",
  "sub": "userId123345",
  "aud": "http://xyz.com",
  "exp": 1621996466, //Valid token for 30mins
  "iat": 1621994666,
  "name": "John Wick",
  "firstName": "John",
  "lastName": "Wick",
  "gender": "male",
  "birthdate": "1972-10-22",
  "email": "johnwick@abc.com"
}
```

## AccessToken

这个名字表明了它的用途。它们提供对受应用程序保护的资源的访问。这些不携带大量信息，但根据权利要求中关联的 ***唯一标识符*** 明确识别用户。通常包含在“**子**权利要求中的唯一标识符。

您还可以合并“scope”私有声明，该声明定义了使用该令牌可以访问哪些特性。

这些令牌通常寿命很短，不能长期或无限有效。

本文将主要关注这些令牌的生成。

根据 HTTP 标准，这些令牌被认为是“**载体**令牌，理想情况下应该在请求的“**授权**报头中传递。您总是可以在其他自定义标题中传递此信息，但对于本文，我们将坚持标准。

```
//Body Sample Access Token
{
      "iss": "https://abc.com/",
      "aud": "https://xyz.com/",
      "sub": "userId123456",
      "scope": "create modify modifyothers",
      "iat": 1621994666,
      "exp": 1621996466
}
```

## RefreshToken

刷新令牌通常是一次性使用的令牌(取决于实现方式),在访问令牌过期的情况下，访问方使用这些令牌来刷新访问令牌。

这个概念基于 OAuth2.0，我想这不是本文的目的。

# 设置

## 生成公钥/私钥

我们将使用 RSA 算法进行 JWT 创建和签名。

私人密钥

该密钥是发行方的私有密钥。在任何情况下，这些信息都不能与他人分享。如果出现数据泄露或密钥受损，应立即更换。

**使用**:如果您想“解密”某些信息，或者为要与他人共享的内容“创建签名”，可以使用这些选项。

公开密钥

密钥本质上是公开的，可以与各方共享。公开密钥的目的是为了在双方之间建立信任。

持有私钥的一方必须签署信息，接收方必须使用公钥验证信息。类似地，如果消费方需要发送一些他们认为是机密的信息，那么它必须使用公钥加密，并且在这种情况下接收方/发布方必须使用私钥解密信息

现在让我们在本地机器上生成一个密钥对，看看它是如何工作的。启动您的终端并导航到您希望密钥所在的文件夹。执行以下命令来生成密钥对。

```
ssh-keygen -t rsa -b 4096 -m PEM -f dev.key
```

生成密钥时会要求您输入密码，请确保将其留空。

![](img/5b41c423e81887fb5aee7630dac9bf6a.png)

最后，执行下面的命令从生成的文件中提取公钥。

```
openssl rsa -in dev.key -pubout -outform PEM -out dev.key.pub
```

我们有一组密钥对，我们将在应用程序中使用它们来生成令牌。

## 创建具有依赖关系的快速应用程序

让我们快速设置我们的 express 应用程序。我们将使用的软件包如下:

*   **快递**
*   **nodemon**
*   jsonwebtoken —这个库将帮助我们在应用程序中处理和生成 JWT。

返回您的终端，导航到您想要生成应用程序的位置，并执行以下命令

```
npm init
// Go through the terminal wizard process to generate package.json file
npm i -P express nodemon jsonwebtoken
```

## 设置

*   **config/keys** —保存上一步生成的密钥的目录—将上一步生成的密钥复制到该文件夹。
*   **server.js** —作为应用程序的入口点
*   **src/helper.js** 文件 APIs 的助手方法和数据。
*   **src/app . Middleware . js**—保护路由的中间件
*   **src/router.js** —应用程序的路由
*   **src/controller.js** —路由的服务实现。

![](img/5d010a89c7b3b31c177cdb4b0f1f55e8.png)

## 发展

helper.js

```
//File contains sample data and few helper methods that will be used in the application.const fs = require("fs");//Returns the private key
exports.readPrivateKey = () => {
    let filePath = "./config/keys/dev.key"
    try{
        let data = fs.readFileSync(filePath,'utf-8');
        if(!data){
            console.log('Cannot read the private key');
            return
        }
        return data
    }catch(error){
        console.log(error)
    }
}
//Returns the public key
exports.readPublicKey = () => {
    let filePath = './config/keys/dev.key.pub'
    try {
        let data = fs.readFileSync(filePath, 'utf-8');
        if(!data){
            console.log('Cannot read the public key!');
            return
        }
        return data
    } catch(error) {
        console.log(error)
    }
}
exports.items = [
    {
    "id": 1,
    "employee_name": "Tiger Nixon",
    "employee_salary": 320800,
    "employee_age": 61,
    "profile_image": ""
    },
    {
    "id": 2,
    "employee_name": "Garrett Winters",
    "employee_salary": 170750,
    "employee_age": 63,
    "profile_image": ""
    },
    {
    "id": 3,
    "employee_name": "Ashton Cox",
    "employee_salary": 86000,
    "employee_age": 66,
    "profile_image": ""
    },
    {
    "id": 4,
    "employee_name": "Cedric Kelly",
    "employee_salary": 433060,
    "employee_age": 22,
    "profile_image": ""
    },
    {
    "id": 5,
    "employee_name": "Airi Satou",
    "employee_salary": 162700,
    "employee_age": 33,
    "profile_image": ""
    }
];// Sample user object
exports.user = Object.freeze({
    userId: "123456",
    firstName: "John",
    lastName: "Wick",
    fullName: "John Wick",
    email: "johnwick@example.com"
});
```

controller.js

让我们创建将在路由器文件中使用的服务实现。

```
//Import the packages required
const jwt = require("jsonwebtoken");
const {
    items,
    user,
    readPrivateKey
} = require('./helper');
```

获取所有项目

```
//Get All Items Service implementation
exports.getAllItems = (req,res,next) => {
    //Check if the user information is available
    if(!req.user) {
        return res.status(403).json({
            success: false,
            message: "Access denied to the resource!"
        });
    } res.status(200).json({
        success: true,
        data: items
    })
};
```

生成访问令牌

```
//Generate an Accesstoken
exports.getAccessToken = (req,res,next) => {

    // ----------------- TODO STARTS -------------------------
    // User validation must go here to make sure it is a valid user.
    // User can be validated on the basis of credentials that was used to register with the application
    // ----------------- TODO ENDS ------------------------- //Returns the private key for the generating the token.
    let privateKey = readPrivateKey()
    if(!privateKey){
        res.status(500).json({
            success: false,
            message: "Cannot generate the access token. Internal Server Error"
        });
    } // Prepare the payload consisting of the claims
    let payload = {
        id: user.userId
    }

    // Prepare the options for the access token
    // Sets the time of the token to expire in 30mins
    let jwtOptions = {
        algorithm: "RS256",
        issuer: "http://example.abc.com",
        audience: "http://example.xyz.com",
        expiresIn: (30 * 60),
        subject: payload.id,
        jwtid: `${(Math.floor(Date.now))}`
    } //Generate the access token
    let accessToken = jwt.sign(payload,privateKey, jwtOptions); //Add the information to the response header.
    res.header("x-access-token",accessToken)
    res.status(200).json({
        success: true,
        message:"You have been authenticated successfully"
    });
}
```

app.middleware.js

该文件包含一个中间件，它将检查是否为受保护的路由提供了令牌。作为该过程的一部分，它将检查以下内容。

*   签名验证
*   到期验证
*   检查请求资源的实体是否有效。在这种情况下，我们将检查当前作为帮助文件中静态信息的 userId。

```
//Import modules, files and data
const jwt = require('jsonwebtoken')
const {
    readPublicKey,
    user
} = require('./helper')
```

validateToken

该方法确保信息是有效的，并且对为访问而发出的令牌进行了修改。

```
//Verfies the token provided
exports.validateToken = async (req,res,next) => {
    let token; if(req.headers.authorization &&
        req.headers.authorization.startsWith('Bearer')) {
        token = req.headers.authorization.split(" ")[1]
    } //Check if token exists
    if(!token) {
        res.status(403).json({
            success: false,
            message: "Access Denied to the resource!"
        });
        return
    } let publicKey = readPublicKey()
    if(!publicKey) {
        res.status(500).json({
            success: false,
            message: "Internal Server Error. Cannot read the public key"
        });
        return
    } //Verify the token 
    try {
        let tokenInfo = await jwt.verify(token, publicKey);
        console.log(tokenInfo)
        // Check if the token subject matches the user information to make sure it is the correct token.
        // In case of real system, this information will be compared with the information in database.
        if(tokenInfo.id !== user.userId) {
            res.status(403).json({
                success: false,
                message: "Access Denied to the resource!"
            });
            return
        }
        req.user = user;

        //MUST call next for further processing.

        next(); } catch (error) {
        console.log(error)
        res.status(403).json({
            success: false,
            message: "Access denied to the resource!"
        })
    }
}
```

router.js

该文件将登记为用户访问而公开的路由。

```
//Import modules, files and data
const express = require("express");
const router = express.Router()const {validateToken} = require('./app.middleware')const {
    getAllItems,
    getAccessToken
} = require('./controller')router.route('/auth').post(getAccessToken);
router.route('/').get(validateToken, getAllItems);module.exports = router
```

正如您在上面看到的，GET 路由是通过我们前面定义的中间件来保护的。

server.js

该文件将作为我们的应用程序的入口点，并将包含基本的 Express 代码。

```
// Module and Package imports
const express = require('express');
const router = require('./src/router');// App variables
const app = express()
app.use(express.json())//Enable the router in the application
app.use('/api',router);const PORT = 5001//Initialize the server.
app.listen(PORT, () => {
    console.log(`Server running on ${PORT}`);
})
```

## 测试

我将在 POSTMAN 客户机中测试在整个过程中创建的 API，以确保一切按预期工作。

**步骤 1 —在没有访问令牌的情况下调用 GET /api/**

您将收到一个 **403** **禁止状态**和**拒绝访问**错误，因为没有令牌可用于获取对内容的访问。

![](img/816b73ea26ee62e40af0cc98708f4cf9.png)

**步骤 2 —调用 POST /api/auth**

这将生成并返回一个访问令牌。在响应中使用标题，并复制“x-access-token”内容

![](img/282340e1f4819028e35e97ac919a156b.png)

**步骤 3 —修改 GET /api/以携带“承载者”身份验证**

转到授权并选择客户端上的不记名令牌。将在**步骤 2** 中复制的内容粘贴到此处，并再次尝试获取数据。

![](img/f7ec1c21f76787b5c855776465ac873d.png)

**第 4 步—再次申请物品**

正如您将看到的，这一次，如果令牌没有过期，您将收到一个员工列表，该列表在具有 **200 OK** 响应的帮助器中被列为静态数据

![](img/852cf74ec6e421f228f0d70015a573f1.png)

# 最后的想法

我想您现在已经对如何使用 JWT 令牌在应用程序中启用身份验证有了足够的了解。我希望这篇文章已经帮助你很好地理解了基础知识。以防你需要我做些修改。请在下面留下评论。

希望你喜欢这篇文章。编码快乐！干杯:)

[链接到示例代码 GitHub](https://github.com/cmgabriel/SampleAppNodeJSJWT)