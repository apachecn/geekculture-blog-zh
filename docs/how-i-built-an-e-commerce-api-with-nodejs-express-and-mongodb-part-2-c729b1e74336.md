# 我如何用 NodeJs、Express 和 MongoDB 构建电子商务 API(第 2 部分)

> 原文：<https://medium.com/geekculture/how-i-built-an-e-commerce-api-with-nodejs-express-and-mongodb-part-2-c729b1e74336?source=collection_archive---------3----------------------->

![](img/3ccf40509849290ebc6614eeb62ff827.png)

这是本系列的第二部分。

在第一部分中，我们建立了我们的项目，并解释了我们将使用的各种工具。

在这一部分，我们将构建模型、**购物车、商品和用户。**

如果你没有看过第一部分，这里有链接。

[](/geekculture/how-i-built-an-e-commerce-api-with-nodejs-express-and-mongodb-7b42b5253ffb) [## 我如何用 NodeJs、Express 和 MongoDB 构建电子商务 API

### 第 1 部分:设置项目

medium.com](/geekculture/how-i-built-an-e-commerce-api-with-nodejs-express-and-mongodb-7b42b5253ffb) 

我们使用 MongoDB 作为我们的数据库，我们将使用 Mongoose 与它进行交互。

为了保持有序，我们将在根目录下创建一个名为 **models、**的新文件夹，在里面，我们创建三个文件， **User、Cart、Item、** with”。js”扩展名。

**用户模型。**

我们首先导入所有需要的模块，**mongose、validator、bcrypt 和 jwt。**

`const mongoose = require(‘mongoose’)
const validator = require(‘validator’)
const bcrypt = require(‘bcryptjs’)
const jwt = require(‘jsonwebtoken’)`

然后我们创建我们的用户模式。这将成为创造新用户的一种方式。

```
const userSchema = new mongoose.Schema({
name: {
   type: String,
   required: true,
   trim: true,
   lowercase: true
 },
email: {
   type: String,
   required: true,
   unique: true,
   lowercase: true,
     validate( *value* ) {
           if( !validator.isEmail( value )) {
                throw new Error( ‘Email is invalid’ )
                 }
            }
  }
password: {
    type: String,
    required: true,
    minLength: 7,
    trim: true,
    validate(*value*) {
       if( value.toLowerCase().includes(‘password’)) {
       throw new Error(‘password musn\’t contain password’)
      }
   }
},
tokens: [{
  token: {
  type: String,
  required: true
    }
  }]
}, {
timestamps: true
})
```

该模式中有各种字段，这将是每个用户都有的参数。

我会花时间来解释每一个和他们做什么。

1.  **姓名** —该字段保存每个用户的姓名。它属于字符串类型，并且是必填字段，因此除非您提供此字段，否则无法注册。我们正在修剪，以消除尾随和前导空格，并转换为小写。
2.  **电子邮件** —该字段包含用户的电子邮件。它是必填字段，也是一个字符串。我们希望我们的电子邮件是唯一的，所以我们将其设置为 true，我们还希望检查电子邮件的格式是否正确，所以我们使用 validator 的 **validate** 方法进行检查。如果电子邮件的格式不正确，我们会抛出一个新的错误。
3.  **密码** —包含用户密码。它的类型是 string，是必需的，我们也希望它的长度超过 7 个字符，所以我们包含了 minLength 属性。我们还在这里使用 validate 来检查密码是否包含“password ”,如果包含，则抛出一个错误。
4.  **令牌** —令牌是包含 jwt 生成的令牌的对象数组。在接下来的部分中，我们将使用这些令牌来认证用户。
5.  **时间戳** —这将自动为我们的模型创建一个 createdAt 和 updatedAt 字段。

接下来，我们创建模型并导出它

```
const User = mongoose.model(‘User’, userSchema)
module.exports = User
```

**物品型号**

我们需要创建的下一个模型是*项目*模型。在这里，我们将为商店中用户将要购买的商品设计模式。我们将保持我们的项目模式简单，现在将不包括*图像。你当然可以添加产品图片或任何你想添加的额外字段。*

我们将在 models 文件夹内名为 ***Item.js*** 的文件中构建我们的*项*模型。我们首先需要*mongose*并创建模式对象。

```
const mongoose = require(‘mongoose’)
const ObjectID = mongoose.Schema.Types.ObjectId
```

ObjectID 是在 mongoDB 中创建的每个文档的唯一标识符。我们需要用它来将每个创建的条目与创建它的用户联系起来。

接下来，我们设计模式

```
const itemSchema = new mongoose.Schema({owner : {
   type: ObjectID,
   required: true,
   ref: 'User'
},
name: {
   type: String,
   required: true,
   trim: true
},
description: {
  type: String,
  required: true
},
category: {
   type: String,
   required: true
},
price: {
   type: Number,
   required: true
}
}, {
timestamps: true
})
```

“所有者”字段包含创建该项目的用户的 id。它的类型是 ***ObjectID*** ，因为我们只想要这个用户的 ID，而 ref 是‘User’。ref 指向我们想要 id 的型号。

项目的名称，它的描述，类别和它的价格是不言自明的。

接下来，我们创建模型并导出它。

```
const Item = mongoose.model('Item', itemSchema)module.exports = Item
```

**推车型号**

我们要构建的下一个模型是购物车模型。购物车包含用户要购买的所有商品。

购物车模型将包括字段，如所有者，项目和总账单。

我们从加载所需的模块开始

```
const mongoose = require('mongoose')const ObjectID = mongoose.Schema.Types.ObjectId
```

然后我们定义我们的模式

```
const cartSchema = new mongoose.Schema({owner : {
  type: ObjectID,
   required: true,
   ref: 'User'
 },
items: [{
  itemId: {
   type: ObjectID,
   ref: 'Item',
   required: true
},
name: String,
quantity: {
   type: Number,
   required: true,
   min: 1,
   default: 1},
   price: Number
 }],
bill: {
    type: Number,
    required: true,
   default: 0
  }
}, {
timestamps: true
})
```

该模式有三个字段——所有者、项目和账单。

**Owner** —顾名思义，包含拥有特定购物车的用户的 id。

**条目** —用户添加到购物车中的所有条目的数组。它还包括一些子字段； **itemId** (加入购物车的物品的唯一 id)**name**(物品的名称) **quantity** (物品的数量，默认为一个)。

**账单—** 购物车的总账单，如果购物车是空的，默认为 0。

现在，我们创建模型并导出它，

```
const Cart = mongoose.model('Cart', cartSchema)
module.exports = Cart
```

既然我们已经完成了模型。我们继续创建我们的路由和身份验证。

下一部分再见。

[点击此处阅读本系列的第三部分](/geekculture/how-i-built-an-e-commerce-api-with-nodejs-express-and-mongodb-part-3-60150d354587)