# Headless WooCommerce & Next.js:设置 Stripe 并创建 Checkout

> 原文：<https://medium.com/geekculture/headless-woocommerce-next-js-set-up-stripe-and-create-checkout-d01333607a66?source=collection_archive---------4----------------------->

![](img/6f5c41db686f5b61564f4d83346084c8.png)

Photo by [Dan Smedley](https://unsplash.com/@nadyeldems?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

这个拼图游戏的最后一块是建立一个 Stripe 帐户，并使用它来处理我们结账时的卡支付。

# 创建条带帐户

首先，你需要注册一个 Stripe 账户，这是免费的，之后你就可以访问开发 API 密钥进行测试。Stripe dashboard 对于开发人员来说非常方便，因为它可以让您看到活动日志，这样您就可以检查事情是否正常工作或为什么不正常工作。Stripe 还提供了一个全面的测试卡列表，您可以使用它来测试各种情况(例如，资金不足、欺诈卡)。

回到开发 API 键。你在寻找‘可公开密钥’和‘秘密密钥’。如果你在仪表板上看不到它，那么浏览菜单选项:开发者-> API 密匙。我将这些键作为环境变量存储在我的应用程序中。可发布的密钥不必是秘密的，所以我用`NEXT_PUBLIC_`作为前缀，但是我确保秘密密钥只在服务器端使用。

# 为 WooCommerce 插件安装 Stripe

WooCommerce 有一个名为“WooCommerce Stripe Gateway”的 Stripe 插件，它为你集成了 Stripe。去你的 WordPress 网站下载这个插件并激活它。

安装并激活插件后，你会在 woo commerce-> Settings-> Payments 下找到 Stripe 设置。在这里，我们希望确保`Enable Stripe`和`Enable Test Mode`的复选框被选中。然后输入你的`Test Publishable Key`和你的`Test Secret Key`。点击屏幕下方的`Save Changes`！

如果你有一个通过 HTTPS 服务的 WordPress 站点，而不是你的 http://localhost，那么按照说明将 webhook 端点添加到你的 Stripe 帐户。这真的很有用，因为它允许 Stripe 与您的 WooCommerce 订单进行交流。对于测试，我们可以省略它，但是对于生产来说，它绝对值得配置。

![](img/26bcd794da920ca09c99ae608e0178d3.png)

Photo by [Meghan Rodgers](https://unsplash.com/@rodgersm22?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 使用条带 SDK

Stripe 有一个 [React 特定的 SDK 和一个关于如何设置它的教程](https://stripe.com/docs/stripe-js/react#setup)。如果这个教程还不够，那么 [Stripe 还有另一个深入的教程](https://stripe.com/docs/payments/integration-builder)，教你如何设置自定义支付流程，包括你需要包含的服务器端和客户端。

如果你想和我一起学习，那么我用了以下几个包:

```
yarn add @stripe/react-stripe-js --dev
yarn add @stripe/stripe-js -- dev
yarn add stripe --dev
```

**条纹元素提供者**

为了安全地捕获卡的详细信息，我们将依靠条纹元素。条纹元素是“提供一种灵活的方式来安全地收集 React 应用程序中的支付信息”的组件。最重要的是，您可以依靠 Stripe 安全地处理敏感数据，而不会接触到您的服务器。

我们最终将有一个结帐表单组件，我们希望在其中使用 Stripe 元素。为了使我们能够使用 Stripe 元素，我们需要用 Elements provider 包装 checkout 表单组件。

注意，我们还使用了`loadStripe`方法来创建一个带有可发布键的 Stripe 对象。这个 Stripe 对象被传递给我们的提供者，因此我们知道我们与哪个 Stripe 帐户相关联。

**条纹元素组件**

我们可以使用许多不同的条带元素组件[和一个流行的组件`CardElement`，它提供了一个一体化组件来捕获所有必要的卡细节。简单。](https://stripe.com/docs/stripe-js/react#available-element-components)

但是，我想向您展示如何使用其他元素来创建不同的 UI。为此，我想使用`CardNumberElement`、`CardExpiryElement`和`CardCvcElement`，这样我仍然可以捕捉这些重要的卡片细节，但我可以灵活地显示它们。

为了在 UI 中给你额外的灵活性，你可以给组件一个`className`,并使用普通的 CSS 样式。这很有用，因为条纹样式对象是有限的。做[检查条纹文档](https://stripe.com/docs/js/appendix/style)看看你能在他们的样式对象中玩什么。

我们现在有了一种安全地捕获卡细节的方法，所以接下来要处理的事情实际上是使用卡来支付某些东西。

**结账流程**

我们有这样一个场景，一个用户在购物车里放了一些东西，他们想结账并用卡购买。

*1。创建条纹支付意向*

我们需要做的第一件事是创建一个条纹支付意向。从根本上来说，我们需要创建一个支付意图的只是`amount`和`currency`。然后，我们使用 Stripe 方法创建一个付款意向，成功返回一个包含`client_secret`的对象。这个`client_secret`就是我们后面完成支付所需要的。

至关重要的是，我们要计算服务器端的数量。这样做的原因是，我们传递到 Stripe 支付意图中的金额不能被轻易篡改。我们的购物车已经包含了 WooCommerce 的产品 id 和数量，所以我使用这些信息从服务器端的 WooCommerce 获取价格，并计算总计。我在`\pages\api`文件夹中创建了`create-payment-intent.ts`，以便创建一个定制的 API 路由。这种创建 API 路由的方法是 Next.js 独有的(但是您可以看到它有多简单)。

重要提示:虽然在上面的代码中没有立即注意到，但是数量是以特定的格式表示的，其中 1.00 表示为 100。这在处理货币价值时很常见。Javascript 有一个恼人的问题，浮点舍入误差，有时会导致计算浮点数时出现不正确的数学结果。在我们的例子中，这可能意味着在某些情况下你可能会损失一便士，这是一个很小的数额，但不应该有任何差异。将 1.00 这样的浮点数转换成 100 这样的整数可以避免这些问题。

在上面的代码中，我希望返回支付意图 ID 和客户机密。尽管我们以后会用到这些，但我们希望避免保存客户端机密，因为恶意用户很容易获取它。

*2。创建 WooCommerce 订单*

我们之前已经介绍了如何创建 WooCommerce 订单，除了我们可以将 Stripe 支付意向 ID 传递到`meta_data`字段之外，这里没有实质性的区别。这样做的原因是，这一特定订单和特定的 Stripe 支付意图之间存在联系。当设置了 webhooks，并且您正在使用 WordPress/WooCommerce 的生产版本时，您可以直接从 WooCommerce 仪表板而不是 Stripe 仪表板退款，因为订单与特定的付款意向相关联，并且 WooCommerce Stripe Gateway 使您能够处理退款。

```
const data: Order = {
  payment_method: "stripe",
  payment_method_title: "Card",
  set_paid: false,
  line_items: lineItems,
  meta_data: [
    {
      key: "_stripe_intent_id",
      value: paymentIntentId,
    },
  ],
};
```

上面的代码是您在将付款意向 ID 作为`meta_data`包含时必须遵守的格式示例。注意它是一个数组中的对象。密钥`_stripe_intent_id`是特定的，值是您之前收到的付款意向 ID。

现在，我们实际上已经将我们的 WooCommerce 订单与我们的 Stripe 支付意向相关联。

*3。确认条纹支付*

我们已经创建了一个条纹付款意向，并指定了我们想要处理的金额。接下来要做的是获取卡的详细信息，并根据支付意图确认支付。

我们已经准备好了条带元素组件来捕获卡的详细信息，那么我们如何实际获取它收集的数据呢？为此，我们使用`useElements`钩子。用`const elements = useElements()`实例化它。

如果您正在使用一体化条纹元素组件`CardElement`，那么您可以通过`const cardElement = elements.getElement(CardElement)`获得卡的详细信息。在我们的例子中，我使用了三个不同的组件:`CardNumberElement`、`CardExpiryElement`和`CardCvcElement`。那么我们如何获得卡的详细信息呢？

其实和用`CardElement`一样简单。我们没有传入`CardElement`，而是传入`CardNumberElement`，Stripe 知道从其他组件获取数据。

我们有来自付款意向的`client_secret`。我们有包含我们的卡细节的`cardElement`。现在我们准备确认付款。

为此，我们使用`useStripe`钩。导入它并用`const stripe = useStripe()`实例化它，然后我们可以使用`confirmCardPayment`方法。

`confirmCardPayment`接受两个参数:客户秘密和支付方式。我们有客户端的秘密，但是我们还没有完整的 Stripe PaymentMethod 对象。

支付方法对象的基础是通过我们之前获得的`cardElement`。这是我们可以传入`confirmCardPayment`的基本支付方式对象。

```
{
  payment_method: {
    card: cardElement,
  },
}
```

当然，您可以根据需要添加更多的细节(包括`receipt_email`、`billing_details`和`shipping`通常很方便)。

```
// create Stripe confirm payment method data
// TODO add more data for Stripe to hold if necessary.  Receipt email is usually a good ideaconst paymentMethod = {
  payment_method: {
  card: elements.getElement(CardNumberElement)!,
  // billing_details: {},
  // shipping: {},
  // receipt_email: ''
  },
};// use Stripe client secret to process card payment method
try {
  const result = await stripe.confirmCardPayment(clientSecret, paymentMethod);
  if (result.error) {
    throw new Error(result.error.message);
  }
  return result;
} catch (error) {
  throw new Error(error);
}
```

一个你称之为`stripe.confirmCardPayment(clientSecret, paymentMethod)`的人，就是这样！该卡已经充值，您已经创建了一个与 Stripe 支付意向相关联的 WooCommerce 订单。

**接下来的步骤**

您可以考虑以下几个步骤。这些是我在代码中包含的，欢迎你来看看我在 GitHub 上是怎么做的。

卡支付表单验证—利用 Stripe 的错误处理功能获得关于卡详细信息是否不正确的实时反馈。

加载指示器——创建 Stripe 付款意向、WooCommerce 订单和确认付款可能需要一段时间，因此使用某种加载指示器锁定表单是个好主意。

重置 Redux Cart —成功完成事务后，重置 Redux Cart 状态。

stripe web hooks——记住，只有当你的 WordPress/WooCommerce 网站在 HTTPS 提供服务时，你才能使用它。但是，这样做是值得的，因为您可以在卡支付成功时得到通知，并使用 webhook 触发一个函数来更新您的 WooCommerce 订单的状态。

GraphQl——你可以使用 graph QL 来代替 WooCommerce REST API，这是非常值得的。您可以在一个查询中获得跨多个 API 端点的所需数据，并高效地获取所需数据，忽略不需要的数据。

WooCommerce 产品——到目前为止，我们探索了简单的产品。但是，您可以尝试产品的变化，看看如何使用它。您将获得一个`variation_id`和一个`product_id`，并且您可能还需要使用带有 [WooCommerce REST API](https://woocommerce.github.io/woocommerce-rest-api-docs/?javascript#retrieve-a-product-variation) 的`/variations`端点来检索变体。

响应式设计——我展示的代码目前只在手机上看起来不错。我们希望添加断点和媒体查询来创建一个响应的应用程序。

# 最后的话

我希望你喜欢向你介绍 Headless WooCommerce 和 Next.js 的这一系列文章。我试图向你展示如何实现一个基本的电子商务平台的精简版本，你可以在上面添加许多层。正如所有事情一样，在前进的道路上不可避免地会有挫折，我可能忽略了这些文章中的一些重要细节。我很乐意帮忙，但是我可以，所以找我，让我们联系。

对于那些坚持到现在的人来说:[这里是我的公共 GitHub 的链接，带有这个 woocommerce-nextjs 例子的代码](https://github.com/leojbchan/woocommerce-nextjs)。

第 1 部分:用 WooCommerce 创建一个本地 WordPress

[第 2 部分:用邮递员](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-woocommerce-test-with-postman-5e7859c65626)建立 WooCommerce &测试

[第 3 部分:设置 Next.js](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-next-js-b8d2193b2688)

[第 4 部分:用 TypeScript 和 Next.js 设置样式组件](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-styled-components-with-typescript-and-next-js-18cc047ccd79)

[第五部分:展示产品并创建订单](https://leojbchan.medium.com/headless-woocommerce-next-js-display-products-and-create-orders-593a6c5aee86)

[第六部分:建立 Redux 状态管理工具包](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-redux-toolkit-for-state-management-c605adda58fe)

[第 7 部分:创建购物车](https://leojbchan.medium.com/headless-woocommerce-next-js-create-a-cart-8a3e49b90076)

[第 8 部分:设置条带并创建检验](https://leojbchan.medium.com/headless-woocommerce-next-js-set-up-stripe-and-create-checkout-d01333607a66)