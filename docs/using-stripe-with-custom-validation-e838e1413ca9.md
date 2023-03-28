# 使用带自定义验证的条带

> 原文：<https://medium.com/geekculture/using-stripe-with-custom-validation-e838e1413ca9?source=collection_archive---------12----------------------->

付款前后

![](img/2cd9993b78954df5bf2892a383a1c744.png)

Photo by [Pickawood](https://unsplash.com/@pickawood?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

最近，我刚刚推出了我的第一个小图标库， [Craft:Glyphs](https://www.craftglyphs.com) ，并集成 Stripe 作为支付网关。也许这是过度考虑，但我想确保该电子邮件帐户还没有注册之前，它去结帐。

所以我的流程是这样的:检查电子邮件是否存在>使用表单中预填的电子邮件进行结帐>如果支付成功，使用电子邮件创建帐户。

要在结账时预填电子邮件，我不能使用“支付链接”方法，必须做一些编码，我将在本文中描述。

# 步骤 0:从条带中获取必要的项目

**0a)Stripe SDK**:Stripe 的[文档](https://stripe.com/docs/development/quickstart)列出了各种语言的信息(向下滚动到“2 安装 Stripe 库”)。我用的是 PHP，在 PHP 标签下显示我们可以通过 composer 或者直接从 Github 获得 PHP SDK。我的是从 Github 得到的。

秘密 API 密匙:我们可以通过**开发者> API 密匙**获得。在**标准密钥**下，点击**显示**获取**密钥**。请注意，**秘密密钥**只能在**服务器端**使用，不得公开可见。看起来是这样的:

```
$stripe_key = 'sk_test_xxxxxxxxxxxxxxxxxxxx........';
```

# 步骤 1:创建产品(和价格)

在主菜单栏上，点击**产品**，然后点击**添加产品**。输入**名称**、**描述**(可选)和**图像**(可选)。

输入**价格信息**，并根据需要创建额外的价格信息。

点击**保存产品**。

# 步骤 2:记下价格 ID

从**产品**产品>产品**概述**页面，点击新创建的产品。向下滚动到**定价**，注意我们将要集成的 **API ID** 。它们看起来像这样:

```
$price_id = 'price_xxxxxxxxxxx';
```

# 步骤 3:生成支付链接

正如我前面提到的，在进入结帐屏幕之前，我还有一个额外的步骤。因为这不在本文的讨论范围内，所以我在这里将它写成伪代码:

```
if (email_is_okay($email)){
   // go to stripe
}
```

在这个 if 中，我们使用 SDK 来创建支付链接。总的来说，代码如下所示:

```
require '[path_to_stripe_sdk]/init.php';if (email_is_okay($email)){ $stripe = new \Stripe\StripeClient($stripe_key); // from step 0 try{
      $session = $stripe->checkout->sessions->create([
         'success_url' => 'success.php',
         'cancel_url' => 'cancel.php',
         'line_items' => [
            ['price' => $price_id, // the price id from step 2
            'quantity' => 1,
            'adjustable_quantity' => [ //optional
               'enabled' => true,
               'minimum' => 1,
               'maximum' => 99,],
            ],
         ],
         'customer_email' => $email, //our user email
         'mode' => 'payment',
         'allow_promotion_codes' => true, //optional
      ]); $url = $session->url;
      // $url is the payment link, we can return this result
   }
   catch(\Stripe\Exception\CardException $e) {
   } catch (\Stripe\Exception\RateLimitException $e) {
   } catch (\Stripe\Exception\InvalidRequestException $e) {
   } catch (\Stripe\Exception\AuthenticationException $e) {
   } catch (\Stripe\Exception\ApiConnectionException $e) {
   } catch (\Stripe\Exception\ApiErrorException $e) {
   } catch (Exception $e) {}}
else{
   // return message that $email cannot be used
}
```

需要注意两件事:

**3a) success_url 和 cancel_url** :这些是用户在结帐屏幕后将被转发到的 url。它不同于条带 webhook，因为它不包含有效负载。然而，我们可以添加一些参数，比如“success.php？param=123 '来处理内部逻辑，如有必要。

**3b)$session- > url:** 这是创建的“支付链接”的 url。我用这个将我的用户转到带有预填电子邮件地址的结帐屏幕。

# 第四步:处理回电

如前所述，success_url 可用于处理支付成功后的逻辑，但它不携带来自 Stripe 的有效载荷。如果我们想要存储像 payment_intent 或 receipt url 这样的参考信息，webhook 回调是必要的。

## **4a)用于多个子站点(可选)**

我想提请注意的一点是(因为这并不明显)webhook 端点是针对整个帐户的。

这意味着，如果有子网站 A 和 B(例如，同一公司帐户下的不同派生)，我们不能让一个 webhook 从站点 A 侦听 payment_intent.succeeded，而让另一个 webhook 从站点 B 侦听同一事件。

我们需要额外的代码来区分回调。

在这种情况下，我从条带支持收到的建议是:

> “最好的选择是检查一次成功支付的事件，看看哪些事件有你需要的信息。”

然后，我们将需要像产品支付的信息，以便在不同的子网站触发相应的逻辑。不同的条带事件返回不同的信息，因此，我们需要检查每个事件，以找到最合适的候选项。

我具体是怎么做的，是让一个测试脚本将所有的事件代码发送到我的数据库。

```
try {
   $event = \Stripe\Webhook::constructEvent(
      $payload, $sig_header, $endpoint_secret
   ); //insert event code into the db
   $query = 'insert into db.stripe_test (message) value (:msg)';
   $params = [];
   $params[':msg'] = $event->type;
   $stmt = $db->prepare($query);
   $stmt->execute($params);} catch(\UnexpectedValueException $e) {
   *// Invalid payload*
   http_response_code(400);
   exit();
} catch(\Stripe\Exception\SignatureVerificationException $e) {
   *// Invalid signature* http_response_code(400);
   exit();
}
```

将它上传到测试服务器后，创建一个新的 webhook，并附加所有听起来与我们的用例相关的事件。从支付链接完成一次成功的支付(不是在控制台上测试 Webhook！)来激发所有可能的事件。

在我的例子中，我注意到**payment _ intent . successed**等。在我的结果里。然而，这是我附加的事件的一个较小的子集，我认为应该被触发。(所以不要假设每个事件都会被触发！)

现在我们知道了什么事件会被触发，我们可以转到 **Test Webhook** (在 Stripe 控制台中)来逐个触发事件。检查负载并使用包含我们需要的信息的事件。

## **4b)实现 webhook**

实现 webhook 相对简单，并在 [Stripe 文档](https://stripe.com/docs/webhooks)中提供，所以我不会在这里重复。

但要强调的是，这里重要的部分是 webhook 签名:

```
$event = \Stripe\Webhook::constructEvent(
        $payload, $sig_header, $endpoint_secret
);
```

最后是响应代码:

```
http_response_code(200);
```

要获得$endpoint_secret，请转到**开发者> Webhooks** ，并点击 webhook。在 webhook 界面中，在**签名密码**下，点击**显示**。

# 第五步:上线

上线实际上是再次经历上述相同的步骤，但我仍然遇到了错误，因为我错过了一些步骤。因此，我会唠叨，在这里放一个清单:

**5a)获取实时密钥。这只会被披露一次，所以把它放在一个超级安全的地方。**

**5b)在实时控制台中创建产品和价格，并获取其价格 id。**

**5c)在现场控制台中创建一个 webhook。**确保 webhook 的签名密码得到更新。否则支付成功，但不会触发后续逻辑。(即用户支付了费用，但没有收到产品！)

最后，就这些了！谢谢你读到这里，我希望以上对你有所帮助。祝你发射成功！

顺便问一下，我有没有说过我已经启动了一些项目？如果你能在[工艺:符号](https://www.craftglyphs.com)和[工艺:帖子](https://www.craftposts.com)查看它们，我会很高兴的！