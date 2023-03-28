# 我如何用 NodeJs、Express 和 MongoDB 构建电子商务 API(第 3 部分)

> 原文：<https://medium.com/geekculture/how-i-built-an-e-commerce-api-with-nodejs-express-and-mongodb-part-3-60150d354587?source=collection_archive---------1----------------------->

这里，我们将构建处理所有请求所需的 REST API 端点。我们还将添加一些定制的中间件来检查用户是否经过身份验证。

![](img/3ccf40509849290ebc6614eeb62ff827.png)

在本系列的前几部分中，我们已经学习了如何设置我们的项目，并且已经建立了我们的模型。如果你还没有读过这些，你可以在这里补上

[](/geekculture/how-i-built-an-e-commerce-api-with-nodejs-express-and-mongodb-7b42b5253ffb) [## 我如何用 NodeJs、Express 和 MongoDB 构建电子商务 API

### 第 1 部分:设置项目

medium.com](/geekculture/how-i-built-an-e-commerce-api-with-nodejs-express-and-mongodb-7b42b5253ffb) [](/geekculture/how-i-built-an-e-commerce-api-with-nodejs-express-and-mongodb-part-2-c729b1e74336) [## 我如何用 NodeJs、Express 和 MongoDB 构建电子商务 API(第 2 部分)

### 第 2 部分:构建模型

medium.com](/geekculture/how-i-built-an-e-commerce-api-with-nodejs-express-and-mongodb-part-2-c729b1e74336) 

让我们首先在根目录下创建一个新文件夹，并将其命名为 **routers。**在这个文件夹中，我们创建了四个文件，**、用户、商品、购物车和订单。**这四个文件将处理与每个型号相关的所有请求。

我们将从构建用户路线开始。该路由将处理注册、登录和注销请求。

1.  **用户路线**

首先，我们导入 express 和我们的用户模型。我们需要 express 来创建路由器，需要 model 来创建新用户。

```
const express = require('express')
const User = require('../models/user')
```

然后，我们实例化路由器

```
const router = new express.Router()
```

现在，在我们继续创建注册端点之前，我们需要一个系统在每次新用户注册或登录时生成认证令牌。为了让事情有条理，我们将再次打开我们的用户模型文件，并在我们的模型上创建这个函数作为一个方法。

在导出模型之前，将这些

```
userSchema.methods.generateAuthToken = async function () {
   const user = **this** const token = jwt.sign({ _id: user._id.toString()},      process.env.JWT_SECRET)
user.tokens = user.tokens.concat({token})
   await user.save()
   return token
}
```

这被声明为异步函数，因为我们将写入数据库。

下面是代码解释。

在这个函数中，我们将使用 this 关键字访问用户模型的每个实例。所以为了清楚起见，我把这个问题交给了用户。

接下来，我们使用用户的 id 生成一个令牌，并提供我们的 jwt 秘密。然后，我们将这个新令牌与用户的现有令牌(如果存在的话)连接起来。我们保存文件，然后返回令牌。

现在回到我们的用户路由文件，我们准备使用这个方法。

```
*//signup*router.post('/users', async (*req*, *res*) => {const user = new User(req.body)
try {
    await user.save()
    const token = await user.generateAuthToken()
    res.status(201).send({user, token})
} catch (error) {
    res.status(400).send(error)
}
})
```

我们使用" **User"** 模型从请求主体创建一个新用户，然后我们为新用户生成一个令牌，因为我们不希望他们在创建帐户后必须再次登录。

我们发送回 201 状态代码、用户和令牌，或者如果出现任何问题，我们发送回一个错误。

目前，我们的密码被保存为纯文本，我们不希望这样做，因为这会使我们容易受到攻击，所以我们将使用" **bcrypt** "来散列密码。

回到我们的用户模型文件，我们将添加一个函数来检查用户模型中的密码字段是否被修改，并在保存用户之前散列密码。

Mongoose 为我们提供了一个**“pre”**中间件，它在我们指定的任何动作之前运行。在这里，我们将选择“保存”操作。

```
userSchema.pre('save', async function(*next*) {const user = **this** if (user.isModified('password')) {
   user.password = await bcrypt.hash(user.password, 8)
}
  next()
})
```

在保存任何用户之前，该函数运行并检查密码字段是否被修改。如果是，我们使用 bcrypt 散列密码，然后调用 next 执行保存。

在保存文件之前，我们将添加一个静态函数，该函数将根据用户的电子邮件和密码获取用户，我们稍后将在构建登录路径时使用该函数。

```
userSchema.statics.findByCredentials = async (*email*, *password*) => {const user = await User.findOne({ email })
if (!user) {
  throw new Error('Unable to log in')
}
 const isMatch = await bcrypt.compare(password, user.password)
if(!isMatch) {
   throw new Error('Unable to login')
}
   return user
}
```

我们提供 **findByCredentials** 电子邮件和密码，我们找到具有该电子邮件的用户，如果具有该电子邮件的用户不存在，我们抛出一个错误，这将在我们的路由中触发 catch 阻塞。

如果用户存在，我们将提供的密码与散列密码进行比较，如果匹配，我们返回用户，如果不匹配，我们抛出一个新的错误。

我们完全完成的模型现在应该看起来像这样

然后，我们返回到用户路由文件，为登录用户创建一个端点。

```
router.post('/users/login', async (*req*, *res*) => {try {
  const user = await User.findByCredentials(req.body.email,        req.body.password)
  const token = await user.generateAuthToken()
  res.send({ user, token})
} catch (error) {
  res.status(400).send(error)
 }
})
```

我们使用前面定义的 **findByCredentials** 方法来查找用户，如果没有用户使用该电子邮件，catch 块将被触发，我们将发送回错误，但是如果有用户，我们将为该会话生成一个令牌，并发送回用户和令牌。

**登出路线**

注销路由将是受保护的路由，因为只有经过身份验证的用户才能注销。我们需要找出一种方法来知道用户是否登录。

为此，我们必须创建一个定制的中间件。这个中间件需要从前端发送一个令牌，然后通过检查这个令牌是否有效来验证它，然后将用户添加到请求体中。

在根文件夹中，新建一个文件夹，命名为“**中间件”**在这个文件夹中新建一个文件，命名为“auth.js”。在这个文件中，我们放入了以下代码

```
const jwt = require('jsonwebtoken')
const User = require('../models/user')const auth = async(*req*, *res*, *next*) => {
try {
  const token = req.header('Authorization').replace('Bearer ', '')
  const decoded = jwt.verify(token, process.env.JWT_SECRET)
  const user = await User.findOne({ _id: decoded._id, 'tokens.token':token })if(!user) {
throw new Error
}
  req.token = token
  req.user = user
next()
} catch (error) {
res.status(401).send({error: "Authentication required"})
 }
}
module.exports = auth
```

首先，我们需要 jwt 和我们的用户模型。然后，我们尝试从请求头中获取令牌(这必须是在发出请求时从前端发送的)，如果令牌存在，我们尝试用 jwt 验证它是否有效，如果有效，我们使用用户的 ObectID 和令牌找到用户。

如果不存在用户，我们触发 catch 块，否则，我们将令牌和用户附加到请求对象。

既然我们可以验证用户的身份，我们可以继续构建我们的注销路由。

在用户路由文件中，我们添加了以下内容

```
router.post('/users/logout', Auth, async (*req*, *res*) => {try {
    req.user.tokens =  req.user.tokens.filter((*token*) => {
   return token.token !== req.token
  })
    await req.user.save()
    res.send()
} catch (error) {
    res.status(500).send()
}
})
```

由于我们已经将经过身份验证的用户添加到请求对象中，在身份验证中间件运行后，我们将可以访问用户，并且可以从用户模型中的可用令牌中过滤出当前令牌，我们保存用户，他们不再经过身份验证。

**注意:**这只会将他们从当前会话中注销。如果他们在两个不同的设备上登录，另一个设备仍将被验证。为了解决这个问题，我们提供了一个根来注销所有设备。

**全部注销**

```
router.post('/users/logoutAll', Auth, async(*req*, *res*) => {try {
   req.user.tokens = []
   await req.user.save()
   res.send()
} catch (error) {  
   res.status(500).send()
}
})
module.exports = router
```

该路由清除整个令牌数组。

这就是我们用户路线的样子。

2.**物品路线**

我们从导入所需的模块和实例化路由器开始

```
const express = require('express')
const Item = require('../models/item')
const Auth = require('../middleware/auth') const router = new express.Router()
```

我们需要授权，因为只有登录的用户可以创建项目

2.1 **创建新项目**

```
router.post('/items',Auth, async(*req*, *res*) => {try {const newItem = new Item({
    ...req.body,
    owner: req.user._id
})
   await newItem.save()
   res.status(201).send(newItem)
} catch (error) {
res.status(400).send({message: "error"})
}
})
```

我们从请求体中创建一个新的条目，从请求的用户对象中创建所有者，记住，我们是在 auth 中间件中添加的。

我们保存项目并将其发送回去。

2.2 **取一个项目**

```
router.get('/items/:id', async(*req*, *res*) => {try{
   const item = await Item.findOne({_id: req.params.id})
if(!item) {
   res.status(404).send({error: "Item not found"})
}
   res.status(200).send(item)
} catch (error) {
   res.status(400).send(error)
}
})
```

我们使用请求中提供的 id 找到商品，并发回商品，否则，我们发回一个错误

2.3 **获取所有物品**

```
router.get('/items', async(*req*, *res*) => {try {
  const items = await Item.find({})
  res.status(200).send(items)
} catch (error) {
  res.status(400).send(error)
}
})
```

我们通过提供一个空的对象来获得所有的条目。

2.4 **更新一项**

```
router.patch('/items/:id', Auth, async(*req*, *res*) => {const updates = Object.keys(req.body)const allowedUpdates = ['name', 'description', 'category', 'price']const isValidOperation = updates.every((*update*) =>              allowedUpdates.includes(update))
   if(!isValidOperation) {
     return res.status(400).send({ error: 'invalid updates'})
}try {
  const item = await Item.findOne({ _id: req.params.id})
  if(!item){
      return res.status(404).send()
  }
  updates.forEach((*update*) => item[update] = req.body[update])
  await item.save()
  res.send(item)
} catch (error) {
res.status(400).send(error)
}
})
```

这有点棘手。如果我们在请求体上拥有的只是更新，那就简单多了，但是因为我们还有其他的东西，比如用户，我们不得不采用另一种方法。

首先，我们使用 **Object.keys、**获取要更新的字段，并将它们保存在 **updates** 中，然后我们指定可以更新的字段，在我们的案例中是名称、描述类别和价格。

然后，我们遍历 allowedupdates，确保要更新的每个字段都出现在 allowedupdates 中，如果没有，则不允许这样的更新，并抛出一个错误。

然后，我们继续查找要更新的项目，然后更新相应的字段。我们保存项目并将其发送回去。

2.5 **删除一项**

```
router.delete('/items/:id', Auth, async(*req*, *res*) => {try {const deletedItem = await Item.findOneAndDelete( {_id: req.params.id} )
   if(!deletedItem) {
    res.status(404).send({error: "Item not found"})
}
   res.send(deletedItem)
} catch (error) {
   res.status(400).send(error)
}
})
```

我们使用请求中提供的 id 找到并删除该项目。如果项目存在，它将被返回，我们可以检查是否有任何项目被返回。如果是，我们发回该项目，但如果没有找到项目，我们发回一个错误。

接下来，我们导出路由器

```
module.exports = router
```

最终的文件应该如下所示

本教程系列的第 3 部分到此结束。

在下一部分，我们将继续构建我们的路线。

[点击链接阅读本系列的最后一部分](/geekculture/how-i-built-an-e-commerce-api-with-nodejs-express-and-mongodb-part-4-318e3f494611)