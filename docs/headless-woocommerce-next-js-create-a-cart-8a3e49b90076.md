# Headless WooCommerce & Next.js:创建一个购物车

> 原文：<https://medium.com/geekculture/headless-woocommerce-next-js-create-a-cart-8a3e49b90076?source=collection_archive---------18----------------------->

![](img/833b96ed99c9c46e67c410b11151c47d.png)

Photo by [Lesly Juarez](https://unsplash.com/@jblesly?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

如果你已经阅读了这一系列文章，你会知道我们可以在 WooCommerce 中创建订单，而不需要购物车。尽管如此，用户在结账前管理他们的购物车还是很有帮助的，这也是我们为什么要建立一个购物车的原因。

# 选择您的购物车方法

我们需要决定如何处理这个问题，因为 WooCommerce REST API 不包含端点来帮助您管理购物车。

一种选择是创建一个新的 WooCommerce 订单，并调用 API 来更新行项目。然后，购物车页面将从 WooCommerce 订单中提取行项目进行显示。

另一种选择是我们自己管理购物车，然后在用户准备结账时提交行项目来创建 WooCommerce 订单。这是我选择的选项，因为我们想知道的只是产品 id 和数量，我们应该能够跟踪这些信息。

当然，事情从来没有这么简单，因为即使这就是我们对 WooCommerce 订单的全部需求，我们还需要更多的数据来创建我们自己的购物车 UI。例如，我们需要显示价格，而 WooCommerce 将根据产品 ID 和数量自行计算，我们需要在前端进行计算。

这两种选择都有一个权衡，但我宁愿在前端进行一些计算，而不是进行更多的 API 调用。

# 将数据扩展到 Redux

在我们当前的代码中，用户点击`Add to Cart`，所发生的只是一个行项目的有限版本被添加到 Redux。我们跟踪产品 ID 和数量，因为这是我们创建 WooCommerce 订单所需的全部内容。

```
const lineItem = {
  product_id: product.id,
  quantity: 1,
};
```

然而，现在我们正在处理我们自己的购物车，我想将我们保存的内容扩展到 Redux，这样我们至少有名称和价格作为附加数据字段。

```
const lineItem = {
  product_id: product.id,
  quantity: 1,
  name: product.name,
  price: product.regular_price,
};
```

# 创建购物车

最简单的是，我们有一个存储在 Redux 中的行项目数组，我们希望在一个购物车页面中显示这些行项目。我们需要一个函数来计算每个行项目的总数，就像数量和价格相乘一样简单。

然后，我们需要添加一些功能，以便我们可以增加/减少数量，并完全删除该项目。在前一篇文章中，我已经创建了 Redux 动作函数来允许我们管理它。

对于每个行项目，我包括一个`X`按钮来删除该行项目，以及数量两侧的`-`和`+`按钮来减少和增加数量。这很简单，只需将 Redux 动作函数连接到适当的按钮上。

`CartQty`是显示数量的独立组件，两侧有`-`和`+`按钮来减少或增加数量。

上面的`CartItem.tsx`组件用于显示每个行项目，剩下要做的就是在 Redux 中映射行项目并显示每个`CartItem`。

我还创建了一个组件来显示购物车的总额，我认为这是一个不需要我的代码就可以处理的事情。

# 更进一步

这里创建的购物车几乎是您的基本版本。如果您想在将来的会话中检索购物车，该怎么办？目前，在这种结构下这是不可能的。

为了检索您的购物车，首先您需要一些跨会话持久化的数据。与其保存到 Redux，不如直接保存到本地存储/cookies。或者将 WooCommerce 订单 ID 保存到本地存储/cookie 中，并在用户返回时根据订单 ID 获取该订单。

您可能希望在购物车摘要中显示任何折扣和/或税收信息。既然如此，您需要将保存的内容扩展到 Redux，并进行必要的计算。

最终，它可以像您需要的那样复杂，但是请记住，您需要传递给 WooCommerce 订单的只是产品 ID 和数量。

# 更新 WooCommerce 行项目的问题

这个很烦。假设您创建了一个只有一个行项目的 WooCommerce 订单(它同样适用于多个行项目，但我们将以一个为例)。我们传入`product_id`和`quantity`，行项目被成功添加到订单中，我们可以在返回的 JSON 对象中看到行项目及其所有属性。这是行项目的一个简化示例:

```
{
  id: 5, 
  name: Doughnut,
  product_id: 17,
  quantity: 1,
  subtotal: "8.00",
  total: "8.00",
  price: 8
}
```

所以，如果我想为这个特定的 WooCommerce 订单更新相同的行项目，我可以使用带有`/orders/<id>`端点的 PUT 请求，对吗？我知道我要更新的行项目的 ID，所以我可以在请求数据中传递如下内容:

```
{
  id: 5,
  product_id: 17,
  quantity: 2
} 
```

PUT 请求将会成功，但是 subtotal、total 和 price 会发生一些奇怪的事情。通过将数量增加到 2，我们将会看到`subtotal: "16.00"`和`price: 8`。然而，我们得到的却是:

```
{
  id: 5, 
  name: Doughnut,
  product_id: 17,
  quantity: 2,
  subtotal: "8.00",
  total: "8.00",
  price: 4
}
```

小计不更新。相反，价格通过用旧的小计除以新的数量来更新。这对我来说是最奇怪的。

如果您想要更新现有的行项目，那么您必须自己进行小计和总计计算，并将其包含(作为字符串)。类似这样的事情会起作用:

```
{
  id: 5, 
  product_id: 17,
  quantity: 2,
  subtotal: "16.00",
  total: "16.00"
}
```

提醒一下`subtotal`是折扣前的明细行小计。`total`是折扣后的行总数。[这里是开发者文档](https://woocommerce.github.io/woocommerce-rest-api-docs/?javascript#order-line-items-properties)的链接，了解更多信息。

第 1 部分:用 WooCommerce 创建一个本地 WordPress

[第 2 部分:设置 WooCommerce &测试与邮递员](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-woocommerce-test-with-postman-5e7859c65626)

[第三部分:设置 Next.js](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-next-js-b8d2193b2688)

[第 4 部分:用 TypeScript 和 Next.js 设置样式组件](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-styled-components-with-typescript-and-next-js-18cc047ccd79)

[第 5 部分:展示产品并创建订单](https://leojbchan.medium.com/headless-woocommerce-next-js-display-products-and-create-orders-593a6c5aee86)

[第 6 部分:为状态管理建立 Redux 工具包](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-redux-toolkit-for-state-management-c605adda58fe)

[第 7 部分:创建购物车](https://leojbchan.medium.com/headless-woocommerce-next-js-create-a-cart-8a3e49b90076)

[第 8 部分:设置条带并创建检验](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-stripe-and-create-checkout-d01333607a66)