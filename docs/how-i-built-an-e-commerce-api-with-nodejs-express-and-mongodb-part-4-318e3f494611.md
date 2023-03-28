# 我如何用 NodeJs、Express 和 MongoDB 构建电子商务 API(第 4 部分)

> 原文：<https://medium.com/geekculture/how-i-built-an-e-commerce-api-with-nodejs-express-and-mongodb-part-4-318e3f494611?source=collection_archive---------1----------------------->

![](img/3ccf40509849290ebc6614eeb62ff827.png)

本文已经过更新，支持结帐功能。享受

这是本系列的第四部分。如果你没有读过前面的，你可以使用下面的链接回去阅读。

[](/geekculture/how-i-built-an-e-commerce-api-with-nodejs-express-and-mongodb-7b42b5253ffb) [## 我如何用 NodeJs、Express 和 MongoDB 构建电子商务 API

### 第 1 部分:设置项目

medium.com](/geekculture/how-i-built-an-e-commerce-api-with-nodejs-express-and-mongodb-7b42b5253ffb) [](/geekculture/how-i-built-an-e-commerce-api-with-nodejs-express-and-mongodb-part-2-c729b1e74336) [## 我如何用 NodeJs、Express 和 MongoDB 构建电子商务 API(第 2 部分)

### 第 2 部分:构建模型

medium.com](/geekculture/how-i-built-an-e-commerce-api-with-nodejs-express-and-mongodb-part-2-c729b1e74336) [](/geekculture/how-i-built-an-e-commerce-api-with-nodejs-express-and-mongodb-part-3-60150d354587) [## 我如何用 NodeJs、Express 和 MongoDB 构建电子商务 API(第 3 部分)

### 第 3 部分:构建身份验证和路由

medium.com](/geekculture/how-i-built-an-e-commerce-api-with-nodejs-express-and-mongodb-part-3-60150d354587) 

我们继续用推车路线来构建我们的路线

3.**推车路线**

在我们的购物车路由文件中，我们将有三个端点来处理所有与购物车相关的请求，它们是:**获取购物车、**创建购物车和**删除购物车中的商品。**

我们首先需要我们需要的所有模块，并实例化我们的路由器

```
const express = require("express");
const Cart = require("../models/cart");
const Item = require("../models/item");
const Auth = require("../middleware/auth");const router = new express.Router();
```

3.1 **取车**

```
router.get("/cart", Auth, async (*req*, *res*) => {const owner = req.user._id;try {
    const cart = await Cart.findOne({ owner });
if (cart && cart.items.length > 0) {
     res.status(200).send(cart);
} else {
      res.send(null);
}
} catch (error) {
    res.status(500).send();
}
});
```

首先，我们在请求中从用户对象获取 owner 字段，然后使用它来查找该用户的购物车。如果购物车存在并且里面有一个商品，我们发送回购物车，否则我们发送回 null，如果遇到任何错误，它触发 catch 块。

3.2 **创建购物车**

```
router.post("/cart", Auth, async (*req*, *res*) => {
const owner = req.user._id;const { itemId, quantity } = req.body;try {
    const cart = await Cart.findOne({ owner });
    const item = await Item.findOne({ _id: itemId });
if (!item) {
    res.status(404).send({ message: "item not found" });
    return;
}
    const price = item.price;
    const name = item.name;*//If cart already exists for user,*if (cart) {
    const itemIndex = cart.items.findIndex((*item*) => item.itemId ==  itemId);*//check if product exists or not*if (itemIndex > -1) {
    let product = cart.items[itemIndex];
    product.quantity += quantity;
    cart.bill = cart.items.reduce((*acc*, *curr*) => {
       return acc + curr.quantity * curr.price;
   },0)
cart.items[itemIndex] = product;
   await cart.save();
   res.status(200).send(cart);
} else {
   cart.items.push({ itemId, name, quantity, price });
   cart.bill = cart.items.reduce((*acc*, *curr*) => {
   return acc + curr.quantity * curr.price;
},0)
   await cart.save();
   res.status(200).send(cart);
}
} else {*//no cart exists, create one*const newCart = await Cart.create({
   owner,
   items: [{ itemId, name, quantity, price }],
    bill: quantity * price,
});
return res.status(201).send(newCart);
}
} catch (error) {
   console.log(error);
   res.status(500).send("something went wrong");
}
});
```

首先，我们从用户对象获取所有者，并析构请求体以获取 **itemId** 和**数量。**

接下来，我们使用 owner 字段查找购物车，使用 itemid 查找商品。
如果项目不存在，我们将终止流程并返回一个错误。如果是，我们从 item 对象中获取价格**和名称**的信息。****

如果用户的购物车已经存在，我们将尝试获取购物车中商品的索引，如果索引大于-1(即商品已经在购物车中)，我们将获取商品，并按照请求正文中指定的数量增加其数量，然后保存购物车。如果在购物车中找不到产品，我们将它推入数组，同时提供所有必需的字段； **itemid，名称，数量，价格。然后，我们在数组上使用 reduce 方法计算新的账单。**

这使我们回到第一个 if 语句，现在，如果用户没有购物车，我们创建一个新的，计算账单，保存购物车并将其作为响应发送回来。

3.3 **删除购物车中的商品。**

用户可能会选择删除购物车中的商品。我们可以提供如下功能

```
router.delete("/cart/", Auth, async (*req*, *res*) => {const owner = req.user._id;const itemId = req.query.itemId;try {
   let cart = await Cart.findOne({ owner });
   const itemIndex = cart.items.findIndex((*item*) => item.itemId == itemId);
if (itemIndex > -1) {
     let item = cart.items[itemIndex];
     cart.bill -= item.quantity * item.price;
if(cart.bill < 0) {
      cart.bill = 0
}
     cart.items.splice(itemIndex, 1);
     cart.bill = cart.items.reduce((*acc*, *curr*) => {
return acc + curr.quantity * curr.price;
},0)
    cart = await cart.save();
    res.status(200).send(cart);
} else {
    res.status(404).send("item not found");
}
} catch (error) {
   console.log(error);
   res.status(400).send();
}
});
```

我们从用户对象获取所有者，但是 itemid 作为查询字符串传递。

我们找到购物车，并检查购物车中是否存在要删除的商品。如果是，我们从总账单中减去该商品的总价，并将其从购物车中的商品数组中删除，我们重新计算总账单，保存购物车并将其作为响应发送回去。

我们最终的购物车路线应该是这样的

**4.0 订单路线**

**4.1 获得订单**

```
router.get('/orders', Auth, async (req, res) => {const owner = req.user._id;try {
const order = await Order.find({ owner: owner }).sort({ date: -1 });
res.status(200).send(order)
} catch (error) {
res.status(500).send()
}
})
```

首先，我们在请求中从用户对象中获取 owner 字段，然后使用它为该用户查找订单。如果订单存在，我们将它们按降序返回，否则我们将返回一个 404，表明没有找到订单，如果遇到任何错误，它将触发 catch 块。

**4.2 结账**

接下来，我们有结帐功能，我们将使用 [Flutterwave SDK](https://developer.flutterwave.com/) 设置结帐功能。

如果您没有帐户，请转到上面提供的链接来设置您的帐户并获取您的测试密钥，因为这将是继续操作所必需的。

我们从身份验证中间件获得所有者字段，从请求体获得有效负载。这是有效载荷的样子。

您可以从您的 flutterwave 仪表板复制您的“enckey”加密密钥、flutterwave 公钥和私钥。

接下来，我们使用用户的 id 找到用户的购物车。如果购物车存在，我们用账单和用户详细信息更新有效负载。然后，我们使用提供的详细信息从卡上扣费。

一旦收费成功，我们就使用购物车中的商品和总账单创建订单。由于不再需要，购物车将被删除。然后订单被发送给客户。

这是订单路线的样子

这是整个系列的总结，我们已经建立了一个基本的电子商务 API，您可以添加更多的功能。

这里是 Github 上这个项目的链接，以及用于测试目的的[文档](https://documenter.getpostman.com/view/11784799/UVJhDEyt)的链接。随意克隆和修改。

[](https://github.com/breellz/e-commerce-api) [## GitHub - breellz/e-commerce-api:一个电子商务 web API。产品、购物车和订单上有完整的 CRUD

### 这是一个用 NodeJs 构建的电子商务 API。它的特点是认证，对产品，订单，购物车完全 CRUD 能力…

github.com](https://github.com/breellz/e-commerce-api) 

我希望你学到了一些新东西。

谢谢你看完。