# AWS 跨帐户用户池迁移—无缝登录

> 原文：<https://medium.com/geekculture/aws-cross-account-user-pool-migration-seamless-login-2002b5c726d2?source=collection_archive---------6----------------------->

![](img/106ddb133037363768e673dea7b2fef2.png)

AWS 引入了两种方法将现有的 cognito 用户池迁移到新的用户池

1.  使用用户迁移 Lambda 触发器将用户导入用户池(无缝用户迁移)
2.  将用户从 CSV 文件导入用户池

在第一种方法中，我们可以通过触发用户迁移 lambda 触发器，在用户首次登录时将用户迁移到新用户池。在这种方法中，用户可以使用用于登录旧用户池的相同的现有用户密码。登录和忘记密码都会触发迁移 Lambda 触发器。这是一个无缝的用户迁移。

在第二种方法中，我们可以通过上传包含所有用户的用户配置文件属性的 CSV 文件来批量迁移用户。在这种方法中，用户必须在首次登录时重置密码。

本文将详细讨论第一种方法。

用户迁移 lambda 的基本场景是，它将承担旧用户池帐户中的角色，以便使用旧用户池对用户进行身份验证。

用户迁移 lambda 的基本场景是，它将承担旧用户池帐户中的角色，以便使用旧用户池对用户进行身份验证。

1.  用户尝试登录或验证新用户池
2.  如果在新用户池中找不到用户，将触发用户迁移 lambda 函数
3.  用户迁移 lambda 函数承担旧用户池帐户中的角色
4.  用户迁移 lambda 函数使用交叉帐户承担角色对旧用户池中的用户进行身份验证，并获取用户的属性。
5.  成功通过身份验证的用户被迁移到新用户池

> **先决条件:**

需要有一个使用**AWS CognitoIdentityServiceProvider*****adminInitiateAuth***或 ***initiateAuth*** 的登录方法。当在用户池中找不到用户时，这两种登录方法将触发迁移 lambda 功能。

例如:-

adminInitiateAuth.js

或者

initiateAuth.js

`*AdminInitiateAuth*`支持以下认证流程:

*   用户 _ 服务请求 _ 授权
*   刷新令牌身份验证
*   自定义身份验证
*   管理 _ 无 _ 服务 _ 授权
*   用户密码认证

`*InitiateAuth*`支持以下认证流程:

*   用户 _ 服务请求 _ 授权
*   刷新令牌身份验证
*   用户密码认证
*   自定义身份验证

> **配置:**

在整个配置中，将使用以下符号:

“旧用户池”—用户从中迁移的现有用户池，也是 AWS 帐户 11111111111 中的源用户池

“新用户池”——用户将迁移到的新用户池，也是 AWS 帐户 2222222222 中的目标用户池

**旧用户池配置:**

***第一步*** *:*

为管理 API 启用用户名密码验证以进行验证(ALLOW_ADMIN_USER_PASSWORD_AUTH)。这使得程序化的用户认证更加容易。用户迁移后应禁用此选项，以强制执行不通过网络发送密码的安全 SRP 流。

1.  登录 Cognito 用户池控制台，在左侧导航菜单中，单击“App Clients”。
2.  在应用程序客户端页面上，单击将用于验证用户的应用程序客户端的“显示详细信息”按钮。
3.  确保选择了“为管理 API 启用用户名密码验证以进行验证(ALLOW_ADMIN_USER_PASSWORD_AUTH)”选项。
4.  如果进行了任何更改，保存应用程序客户端更改。

或者，我们可以使用 CLI 命令:

***$ AWS cogn ITO-IDP update-USER-pool-client-USER-pool-id<value>-client-id<value>-explicit-AUTH-flows ALLOW _ ADMIN _ USER _ PASSWORD _ AUTH***

***第二步*** *:*

创建一个可以用来对现有用户进行身份验证的角色。

1.  登录 IAM 控制台，在左侧导航菜单中选择“角色”,然后选择“创建角色”。
2.  选择“另一个 AWS 帐户”角色类型。
3.  对于帐户 ID，键入新的用户池帐户 ID。在这种背景下，会是 22222222222。
4.  选择“下一页”按钮，浏览下一页，直至查看页面。
5.  在“审阅”页面上，添加角色名称。对于本指南，它将是“CognitoCrossAccountMigrationRole”。
6.  选择“创建角色”。
7.  一旦创建了角色，就授予角色进行 API 调用的权限— *adminInitiateAuth* 和 *adminGetUser* 。为此，请使用以下策略向该角色添加内联策略:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "cognito-idp:AdminInitiateAuth",
                "cognito-idp:AdminGetUser"
            ],
            "Resource": "*"
        }
    ]
}
```

为简单起见，该策略允许 API 调用所有资源。我们可以通过指定旧用户池 ARN 来限制旧用户池的资源。或者，我们还可以将角色的信任策略主体限制为新用户池的执行角色 ARN。

**新用户池配置:**

***第一步:***

启用基于用户名和密码的身份验证(ALLOW_USER_PASSWORD_AUTH)。这一步是将用户密码传递给触发的 Lambda 函数所必需的，以便可以使用用户身份验证凭证对旧用户池进行身份验证，在本例中，旧用户池是帐户 11111111111 中的旧用户池。

迁移后，建议禁用 ALLOW_USER_PASSWORD_AUTH 选项，以便验证用户的唯一选项是不通过网络发送密码的安全 SRP 流..

1.  登录 Cognito 用户池控制台，在左侧导航菜单中，单击“App Clients”。
2.  在应用程序客户端页面上，单击将用于验证用户的应用程序客户端的“显示详细信息”按钮。
3.  确保选择了“启用基于用户名和密码的身份验证(ALLOW_USER_PASSWORD_AUTH)”选项。
4.  如果进行了任何更改，保存应用程序客户端更改。

或者，我们可以使用 CLI 命令:

***$ AWS cogn ITO-IDP update-USER-pool-client-USER-pool-id<value>-client-id<value>-explicit-AUTH-flows ALLOW _ USER _ PASSWORD _ AUTH***

***第二步:***

创建用户迁移 Lambda 函数，该函数将承担旧用户池帐户中的角色并迁移用户属性。

1.  登录 Lambda 控制台，在左侧导航菜单中，单击“功能”。
2.  点击“创建功能”按钮创建一个新功能
3.  在 create function 页面上，选择“Author from scratch ”,在 Basic information 部分下，给它一个函数名，并选择 Node.js 16.x 作为运行时。我们假设函数名为“migrationLambdaFunction”。然后点击“创建功能”按钮。
4.  在 Lambda 函数的“代码”选项卡上，将默认函数代码替换为以下代码:

总之，代码需要 UserMigration_Authentication 触发源的用户名和密码，或者 UserMigration_ForgotPassword 触发源的用户名。返回有效用户的属性。这些属性用于迁移用户。

index.js

createNewUser.js

注意:如果你的 app 客户端有秘密，请使用“ [withAppClientSecret](https://github.com/shamilasallay/aws-cross-account-userpool-migration/tree/main/withAppClientSecret) ”中附带的 Lambda 函数来代替。

5.用有效值替换以下值:

> **role arn**’—将采用我们之前设置的旧用户池帐户角色 ARN。例如:-arn:AWS:iam::11111111111:role/cognotocrossactionmigrationrole
> 
> **区域**’—旧用户池区域。例如:-'美国东部-1 '
> 
> **用户池 ID**’—旧用户池 id。例如:-'美国东部-1 _ 示例'
> 
> **app clientID**’—旧用户池 app 客户端 id。例如:-' t 8 pkkgexmapleq 1t 1 vex sampler '
> 
> **LAMBDAROLEARN**’—migration lambda function 角色的角色 ARN。例如:-arn:AWS:iam::*222222222222*:角色/服务-角色/迁移 LambdaFunction-role-gbcihgtr
> 
> **'*λrole*'**-migrationλfunction 角色的角色名称。例如:-migrationLambdaFunction-role-gbcihgtr
> 
> **'*DESTINATIONUSERPOOLID*'**-新用户池 ID。例如:-'美国东部-1_fgythsng '

6.点击“部署”

7.授予 Lambda 函数执行角色权限，以承担旧用户池帐户中的角色。lambda 执行角色可以通过导航到“Permissions”选项卡找到(新控制台中的*:配置>权限*)。角色名称位于“权限”选项卡的“执行角色”部分。单击角色名称直接转到 IAM 控制台上的角色设置，并向该角色添加内联策略以承担跨帐户迁移角色:

```
{
"Version": "2012-10-17",
"Statement": [
    { 
       "Sid": "PermissionToAssumeRole",
       "Effect": "Allow",
       "Action": "sts:AssumeRole",
       "Resource":    "arn:aws:iam::111111111111:role/CognitoCrossAccountMigrationRole" }
  ]
}
```

创建另一个内联策略来授予 cognito 权限:

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "VisualEditor0",
            "Effect": "Allow",
            "Action": [
                "cognito-idp:AdminCreateUser",
                "cognito-idp:AdminSetUserPassword"
            ],
            "Resource": "*"
        }
    ]
}
```

最后，允许 lambda 信任策略主体承担 lambda 角色。

```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::222222222222:role/service-role/migrationLambdaFunction-role-gbcihgtr",
                "Service": "lambda.amazonaws.com"
            },
            "Action": "sts:AssumeRole"
        }
    ]
}
```

确保将策略中的资源替换为具有正确 AWS 帐户 ID 的正确角色 ARN。

***第三步:***

在用户迁移触发器选项中设置 Lambda 函数

1.  登录 Cognito 用户池控制台，在左侧导航菜单中，单击“Triggers”。
2.  在触发器页面上，将“用户迁移”设置为刚刚在新用户池帐户中创建的 Lambda 函数。在这个例子中，它被命名为***migration lambda function***。

注意:如果没有找到创建的 lambda 函数，请确保创建的 lambda 函数与新用户池位于同一区域。您可能还想重新加载触发器页面。

3.保存更改。

> ***注***

1.  在无缝用户迁移中，确保用户不会收到任何电子邮件邀请或电子邮件或手机号码的验证码非常重要。因此，MessageAction 被设置为 ***SUPPRESS*** ，如果在 cognito 用户池中启用了验证，则需要将其移除。在新用户池中转至 cognito 用户池，在左侧导航菜单中单击 **MFA 和验证** ，并在**中选择 ***无验证*** 您要验证哪些属性？**
2.  在新用户池中选择 App 客户端安全配置中的 ***遗留*** 当登录函数返回 *UserNotFound* 错误时触发用户迁移 Lambda

这些配置需要在用户迁移期后回滚。

github repo:—[https://github . com/shamilasallay/AWS-cross-account-user pool-migration](https://github.com/shamilasallay/aws-cross-account-userpool-migration)