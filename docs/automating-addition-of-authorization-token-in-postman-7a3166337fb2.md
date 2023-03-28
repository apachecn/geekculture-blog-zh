# 在 postman 中自动添加授权令牌

> 原文：<https://medium.com/geekculture/automating-addition-of-authorization-token-in-postman-7a3166337fb2?source=collection_archive---------17----------------------->

![](img/c9fc9f4fc367d3e37b40ffd80762e858.png)

Postman

在本文中，我将展示如何设置 postman 集合，以便在向安全端点发出请求时自动包含授权令牌。

# 编辑收藏

首先，我假设您已经创建了一个集合。我创建了一个叫做*的介质*。接下来，我们继续编辑这个集合。这会把我们带到邮差的这个地方。

![](img/12e0957e9c8e4e0a262320f5c91c895d.png)

Collection details

我们从变量选项卡开始编辑。我们添加一个新的变量叫做*令牌。*我们计划在用户通过身份验证并收到回复令牌后，自动更新该变量。该值是随后将作为值在不记名令牌中传递的值。

现在，我们只将*的任意值*作为初始值

![](img/4dc0ae23abc05c00d1e3033fca97f7e3.png)

Adds new variable to collection

接下来，我们返回到授权选项卡，将类型更改为*不记名令牌。F* 或令牌值，而不是简单地放置一个原始令牌，我们宁愿输入我们之前创建的集合变量。我们通过在*标记*的两边使用双花括号来做到这一点。这将使*不记名令牌*的值总是从我们创建的令牌变量中选取。

正如你所看到的，我们几乎完成了我们的实现，剩下的工作就是找到一种方法，一旦我们有了新的令牌，就更新令牌变量的值。

![](img/ab01ee48480cb97c10f9f0b49f681753.png)

Sets authorization type and its value

# 样本响应数据

对于这个例子，我们有一个 auth 端点，它进行身份验证并返回带有这种格式数据的响应。

```
{"message": "User sucessfully login","data": {"token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOiI4OThiN2UwNy01MmM3LTQ3ZWMtOTM5OS02NGNiMzE4MGExODEiLCJyb2xlIjoxLCJlbWFpbCI6ImNoaWJ1aWtlQHNwbGVldC5uZyIsImlhdCI6MTYyMjEyMDM3MiwiZXhwIjoxNjIyMTUwMzcyfQ.W8jMnWaOpNYJPWTU1je4h1br92XuCKSlYyrklHmzL5o"},"statusCode": 200}
```

我们需要将头中返回的对象中的这个令牌值传递给其他受保护并需要传递令牌的端点。

# 在集合头中设置标记

在 *Tests* 选项卡中，我们将添加代码片段，使我们能够从响应中提取令牌并将其存储在集合令牌变量中。

为此，我们向端点发送一个带有我们的主体参数的 post 请求来验证我们。

![](img/4b149b2439456b8f5730d76b7d379e0b.png)

Adds code snippet to extract token

# 试液

一切就绪后，我们单击 send 按钮，这将返回数据对象和状态代码 200。

为了检查我们的代码片段是否有效，我们返回到集合变量来查看 token 和 wolla 的当前值！集合变量标记已用返回的标记更新。

![](img/5120da1b23c3e510ff172d4eb8bece2f.png)

Token variable updated

# 测试安全路由

为了在其他受保护的端点中测试这一点，我们需要做最后一件事。

为了让 token 有一个真实的来源，我们将 Authorization 中的类型设置为 **Inherit auth from parent。**

现在，每当发送请求时，该路由和该集合中的任何其他安全路由的报头中总是具有当前令牌。酷吧？耶！

![](img/019d48a4f609d052165eee93adaad9f6.png)

Set endpoint to use collection authorization

这就是将我们的令牌自动添加到我们的头请求的全部内容。这不仅节省了我一些宝贵的时间，而且实施起来很有趣，也很有收获，哈哈。

玩得开心点！