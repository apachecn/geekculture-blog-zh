# 中如何使用 ActionFilter。NET 核心来保持我的控制器干净

> 原文：<https://medium.com/geekculture/how-i-use-an-actionfilter-in-net-core-to-streamline-authorization-f12acbea7788?source=collection_archive---------1----------------------->

![](img/19861382d64a7ce183953137b4d82dd3.png)

Photo by [Stephen Kraakmo](https://unsplash.com/@srkraakmo?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

# 什么是过滤器？

> ASP.NET 核心中的过滤器允许代码在请求处理管道中的特定阶段之前或之后运行。

中有五种类型的过滤器。网络核心

*   **授权** —首先运行，确定用户是否被授权
*   资源—在授权之后但在所有其他管道之前运行
*   **动作** —在调用方法之前或之后立即运行代码
*   异常—处理异常的全局策略
*   结果—在操作成功执行之前和之后运行

如果你不了解过滤器，我鼓励你在这里学习更多的。对于今天的这篇文章，我们将使用授权和动作过滤器来自动合并控制器中的 UserId，以便于访问。

# 目标—将重复的授权代码抽象到过滤器中

## 抽象 ApiController

```
[ApiController]
[Authorize] // Uses authentication scheme to determine user
[ActionFilter] // We will implement this below
public abstract class ApiController : ControllerBase
{
   public string UserId { get; set; }
}
```

我们在这里做的第一件事是创建一个抽象的基本控制器。我们这样做是为了将这个 ActionFilter 应用于任何将从此类继承的新控制器。它还将封装 UserId，这样我们就不必在我们创建的每个控制器中添加 UserId 字段。还要注意类上面的[ActionFilter]属性，这就是它的作用。NET 为任何继承的类运行此筛选器。

控制器上的[Authorize]属性将决定用户是否有权调用控制器或控制器方法。常见的身份验证方案包括 Cookies 和 JWT，但是对于本文，我们将假设您已经实现了该中间件。

## 动作过滤器

现在我们有了带有 ActionFilter 属性的控制器，让我们写出过滤器本身的代码。

```
public class ActionFilter : Attribute, IActionFilter
{
   public void OnActionExecuted(ActionExecutedContext context) {}

   // Pull the user ID on each request
   public void OnActionExecuting(ActionExecutingContext context
   {
      var c = context.Controller as ApiController;
      c.UserId = c.User.FindFirstValue(ClaimTypes.NameIdentifier);
   }
}
```

这个 ActionFilter 继承自 Attribute(允许我们将其用作属性[ActionFilter])和 IActionFilter，后者要求我们实现两个方法，OnActionExecuted 和 OnActionExecuting。OnActionExecuted 在动作完成后运行*，而 OnActionExecuting 在*之前运行。*因为我们想在控制器动作中使用 UserId，所以我们将使用后者。*

ActionExecutingContext 作为 OnActionExecuting 中的一个参数提供给我们，从那里我们可以访问控制器本身。我们将控制器转换为 ApiController(我们的抽象类),这样我们就可以设置上面声明的 UserId 字段。在本例中，我使用的是 JWT 身份验证，因此我们可以使用第二行逻辑提取 UserId，并将其设置为 UserId。通过设置这个 Id，我们将在任何控制器方法开始执行之前填充 UserId。

## 将 ApiController 与 ActionFilter 一起使用

既然我们已经设置了这两部分，我们终于可以在应用程序控制器中使用抽象的代码了

```
[Route("api/[controller]")]
public class MyController : ApiController
{
   [HttpGet("user-name")]
   public async Task<IActionResult> GetUserName(
      [FromServices] IUserService userService)
   {
       // Passing the UserId we populated in the Filter
       var userName = await userService.GetUserNameAsync(UserId); return Ok(userName);
   }
}
```

因为我们在 ApiController 基类上使用了[ActionFilter],所以我们能够保持 GetUserName 控制器方法非常干净，只需要两行代码(实际上可能只有一行)。如果我们不使用过滤器，我们将需要调用用户。FindFirstValue(ClaimTypes。NameIdentifier)代码来获取我们的用户 id，这是重复的，很难记住。我们写一次 ActionFilter 就忘了。

## 结论

过滤器是中非常强大的工具。NET Core，我希望我能早点了解它。利用过滤器可以抽象出一堆样板代码，让您专注于重要的代码。

我经常使用的另一个过滤器是 ExceptionFilter，您可以在其中处理异常并根据抛出的异常返回 404 或 500，这可以使您的控制器方法更加整洁！！

如果你做到了这一步，请给我留下一些掌声，并关注我的帐户。编码快乐！