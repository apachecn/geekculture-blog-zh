# Azure 密钥管理最佳实践

> 原文：<https://medium.com/geekculture/key-management-in-azure-bf4c69d011bc?source=collection_archive---------8----------------------->

## 从加密访问密钥到基于身份的访问控制

# 概观

随着我们转向基于云的架构，越来越多的应用和服务需要访问云资源。通常，这种访问通过访问密钥或连接字符串进行认证。这给现代云环境中的密钥管理带来了挑战。该页面从身份验证和授权的角度介绍了保护云资源访问安全的原则和技术。网络是安全访问的另一个重要方面，但这不是本页面的范围。

![](img/dbc6eb4d5bc71939317f5dda2ec2be66.png)

[https://brightlineit.com/understanding-encryption-key-management-businesses/](https://brightlineit.com/understanding-encryption-key-management-businesses/)

# 问题是

服务通常没有用户界面，因此在访问后端资源时使用不同的访问控制机制。传统的方法是使用某种秘密密钥来保护访问。该密钥嵌入在连接字符串中或用于对请求进行签名。任何知道密钥的人都可以访问。这种方法很简单，因此很受欢迎，但它有两个主要缺点。首先，资源提供者可以在允许访问资源之前验证密钥，但是它不知道谁是被授权访问的调用者，因为密钥不与任何身份相关联。第二，这样的密钥往往分布在所有需要访问的涉众之间，因此很容易失去跟踪和控制。密钥通常被写入保存在源代码控制中的文件中，从那里到不安全的存储或者甚至通过不安全的通道(如邮件和即时消息)发送是一条捷径。密钥落入恶意者手中的情况通常难以识别和修复。

# 从钥匙到身份

密钥管理问题可能是所有现代应用程序面临的最常见问题之一。为了解决这个问题，现代应用程序利用身份管理标准和技术的最新进展，用身份来代替加密密钥。OAuth 2.0 和 OpenId Connect 1.0 标准是使我们能够为应用程序提供身份的技术基础，类似于为人类用户提供的身份。使用标识代替密钥有几个优点。首先，使用基于身份的访问控制，可以识别和审核每个请求，以便管理员知道是谁发送的请求。其次，身份的范围受到设计的限制，因此撤销对特定身份的访问比撤销密钥简单得多。第三，通过服务和用户共享相同的访问控制机制，我们可以重用基于角色的访问控制(RBAC)来授权对所有资源的所有访问，而不管客户端是谁——人还是机器。

# Azure 中的身份

Azure 完全采用了从密钥到身份的过渡。今天，几乎所有的 Azure 资源都在其 API、SDK 和门户接口中支持 RBAC。这是通过两个主要的 Azure 组件实现的。 [Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/) ，以及[托管身份](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview)。
Azure Active Directory (AAD)是微软基于云的身份和访问管理服务，负责 Azure 以及其他微软服务和无数独立应用程序的所有身份方面。
托管身份是分配给 Azure 资源(如 Azure 功能或虚拟机)的自动主体。托管身份通过为 Azure AD 中的 Azure 资源提供身份并使用它来获取访问令牌，消除了开发人员必须管理凭据的需求。有两种类型的托管身份:

1.  系统分配的。一些 Azure 服务允许你直接在服务实例上启用托管身份。当您启用系统分配的托管身份时，会在 Azure AD 中创建一个与该服务实例的生命周期相关联的身份。所以当资源被删除时，Azure 会自动为你删除身份。根据设计，只有 Azure 资源可以使用该身份向 Azure AD 请求令牌。
2.  用户分配的。您也可以创建一个托管身份作为独立的 Azure 资源。您可以创建用户分配的托管身份，并将其分配给 Azure 服务的一个或多个实例。对于用户分配的受管身份，身份是与使用它的资源分开管理的。

如果受管身份不可行，[应用程序和服务主体](https://docs.microsoft.com/en-us/azure/active-directory/develop/app-objects-and-service-principals)几乎总是可以用作替代。与托管身份不同，[应用和服务主体](https://docs.microsoft.com/en-us/azure/active-directory/develop/app-objects-and-service-principals)确实拥有在[应用注册](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal)时创建的凭证(例如应用 id 和应用秘密)。有了这些，您的代码可以在服务主体的安全上下文中执行，并且在该上下文中，它能够基于给予主体的角色分配来访问资源。应用程序标识是对用户标识的补充。当相关时(通常在 UI 层)，您将通过实现 [OAuth 2.0 授权或隐式流](https://darutk.medium.com/diagrams-and-movies-of-all-the-oauth-2-0-flows-194f3c3ade85)来认证用户，并在用户的安全上下文中运行，但是在服务层，您可能希望使用托管身份或[应用和服务主体](https://docs.microsoft.com/en-us/azure/active-directory/develop/app-objects-and-service-principals)。

# 基于身份的访问流程。

在基于身份的访问中，您的代码向 AAD 进行身份验证，并为特定资源(例如 Azure 存储)的特定访问请求访问令牌。如果您的代码有一个[托管身份](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/overview)，则不需要凭证。如果使用了[服务主体](https://docs.microsoft.com/en-us/azure/active-directory/develop/app-objects-and-service-principals)，你的代码将必须显示应用 id 和应用密码。如果创建了允许主体访问资源的角色分配，AAD 将颁发一个令牌。这个令牌是一个短命的 [JWT 令牌](https://docs.microsoft.com/en-us/azure/active-directory/develop/access-tokens)，只能用来访问那个特定的资源。您可以通过使用 [JWT.ms](https://jwt.ms/#access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSIsImtpZCI6Imk2bEdrM0ZaenhSY1ViMkMzbkVRN3N5SEpsWSJ9.eyJhdWQiOiJlZjFkYTlkNC1mZjc3LTRjM2UtYTAwNS04NDBjM2Y4MzA3NDUiLCJpc3MiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC9mYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTUyMjIyOS8iLCJpYXQiOjE1MzcyMzMxMDYsIm5iZiI6MTUzNzIzMzEwNiwiZXhwIjoxNTM3MjM3MDA2LCJhY3IiOiIxIiwiYWlvIjoiQVhRQWkvOElBQUFBRm0rRS9RVEcrZ0ZuVnhMaldkdzhLKzYxQUdyU091TU1GNmViYU1qN1hPM0libUQzZkdtck95RCtOdlp5R24yVmFUL2tES1h3NE1JaHJnR1ZxNkJuOHdMWG9UMUxrSVorRnpRVmtKUFBMUU9WNEtjWHFTbENWUERTL0RpQ0RnRTIyMlRJbU12V05hRU1hVU9Uc0lHdlRRPT0iLCJhbXIiOlsid2lhIl0sImFwcGlkIjoiNzVkYmU3N2YtMTBhMy00ZTU5LTg1ZmQtOGMxMjc1NDRmMTdjIiwiYXBwaWRhY3IiOiIwIiwiZW1haWwiOiJBYmVMaUBtaWNyb3NvZnQuY29tIiwiZmFtaWx5X25hbWUiOiJMaW5jb2xuIiwiZ2l2ZW5fbmFtZSI6IkFiZSAoTVNGVCkiLCJpZHAiOiJodHRwczovL3N0cy53aW5kb3dzLm5ldC83MmY5ODhiZi04NmYxLTQxYWYtOTFhYi0yZDdjZDAxMjIyNDcvIiwiaXBhZGRyIjoiMjIyLjIyMi4yMjIuMjIiLCJuYW1lIjoiYWJlbGkiLCJvaWQiOiIwMjIyM2I2Yi1hYTFkLTQyZDQtOWVjMC0xYjJiYjkxOTQ0MzgiLCJyaCI6IkkiLCJzY3AiOiJ1c2VyX2ltcGVyc29uYXRpb24iLCJzdWIiOiJsM19yb0lTUVUyMjJiVUxTOXlpMmswWHBxcE9pTXo1SDNaQUNvMUdlWEEiLCJ0aWQiOiJmYTE1ZDY5Mi1lOWM3LTQ0NjAtYTc0My0yOWYyOTU2ZmQ0MjkiLCJ1bmlxdWVfbmFtZSI6ImFiZWxpQG1pY3Jvc29mdC5jb20iLCJ1dGkiOiJGVnNHeFlYSTMwLVR1aWt1dVVvRkFBIiwidmVyIjoiMS4wIn0.D3H6pMUtQnoJAGq6AHd) 等实用程序来查看 [JWT 令牌](https://docs.microsoft.com/en-us/azure/active-directory/develop/access-tokens)的内容。一旦代码拥有了令牌，它就可以通过将令牌放入请求的授权头(即 Authorization: bearer {token})来使用令牌访问资源。几乎所有当前的 Azure SDKs 都实现了这种模式，并且支持开箱即用的 AAD 认证。您可以随时向您的应用程序添加代码来验证安全主体并获取 OAuth 2.0 (JWT)令牌。为了认证和获取令牌，您可以使用任一个 [Microsoft identity platform 认证库](https://docs.microsoft.com/en-us/azure/active-directory/develop/reference-v2-libraries)或另一个支持 OpenID Connect 1.0 的开源库。然后，您的应用程序可以使用访问令牌来授权针对 Azure 资源的请求

# 作为后备的访问键

可能存在无法使用基于身份的访问的情况，例如应用程序需要访问仅支持连接字符串中嵌入的键的旧数据库的情况。在这些情况下，您将需要使用良好的旧访问密钥，但这并不意味着密钥必须存储在不安全的配置文件中。Azure 有一个专门为这些情况设计的密钥库服务。Azure Key Vault 是一种云服务，为秘密提供安全的存储。您可以安全地存储密钥、密码、证书和其他机密。您将把敏感信息(如连接字符串)作为秘密存储在 vault 中，并通过名称引用它，而不是在应用程序配置文件中对其进行硬编码。 [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/general/) 支持基于身份的访问，因此您的代码可以使用其应用程序或托管身份进行身份验证，获得对 Vault 的访问权限，通过名称请求密码，读取密码值并使用它。(例如，建立到遗留数据库的连接。)

# 按键旋转

钥匙旋转总是一个好的实践，但是它必须被正确地做。如果您轮换一个密钥，然后手动更新每个使用它的应用程序，您将很快面临一场噩梦，并破坏您的代码。 [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/general/) 是一个很好的地方，不仅可以保护你的密钥和秘密，还可以管理密钥的生命周期，并作为密钥的中央存储库，因此一旦轮换，只需进行一次更新。下面[教程](https://techcommunity.microsoft.com/t5/azure-architecture-blog/managing-and-rotating-secrets-with-azure-key-vault-managed/ba-p/1800612)详细讲解钥匙旋转自动化流程。

# 威胁分析

对于访问控制和加密操作，密钥是应用程序安全实现中的主要元素。根据我们实施的关键管理策略，可能存在不同的威胁。例如，如果存在除了在应用程序配置中存储密钥之外别无选择的情况，那么密钥失窃的威胁就变得真实了。作为[安全开发生命周期](https://www.microsoft.com/en-us/securityengineering/sdl) (SDL)的一部分，我们执行[威胁建模](https://www.microsoft.com/en-us/securityengineering/sdl/threatmodeling)流程，在该流程中，我们识别威胁并评估相关风险。记录与密钥管理特别相关的威胁并找到尽可能降低风险的缓解措施非常重要。

# 缓解示例

一种常见的缓解策略是减少攻击媒介。例如，AzureWebJobsStorage 是一个 Azure 函数配置设置，它包含 Azure 存储帐户的连接字符串。当使用除 HTTP 以外的触发器时，它是必需的，并且不支持基于身份的访问。在这个例子中，我们需要在配置中包含一个访问键。为了降低风险，我们通过创建一个专用于 Azure function 操作临时数据的独立存储帐户，将操作数据与业务数据隔离开来。我们的业务数据将存储在另一个存储帐户/数据湖中，对它的访问将基于身份。

另一种常见的模式是停车场模式。具有较低安全级别的应用程序(例如，它们在配置中包含访问密钥)只允许写入暂存存储帐户。他们没有访问业务数据湖的权限。在暂存帐户(即较低的信任级别)中，我们为每个应用程序创建一个停车场区域，应用程序可以将数据推送到该区域。另一个服务将监视停车场(blob 触发器)并验证每个传入的文件。通过验证的文件将被复制到可信业务数据湖，然后从停车场移除。

# 结论

*   尽可能使用基于身份的访问控制，而不是加密密钥。
*   将密钥和机密存储在托管密钥库服务中。使用 RBAC 访问模型控制权限。
*   经常轮换密钥和其他机密。在机密生命周期结束时或机密已经泄露时替换机密。
*   记录与您的关键管理策略相关的威胁。

**最后一点:**为了能够继续发布新故事，Medium 现在要求作者拥有最低数量的关注者，所以请帮我继续发布，并按下我名字旁边的“关注”按钮。

# 更多资源

[Azure 中的密钥和秘密管理考虑事项](https://docs.microsoft.com/en-us/azure/architecture/framework/security/design-storage-keys)

[Azure 活动目录中的应用和服务主体对象](https://docs.microsoft.com/en-us/azure/active-directory/develop/app-objects-and-service-principals)

[OAuth 2.0 所有流程的图表和影片](https://darutk.medium.com/diagrams-and-movies-of-all-the-oauth-2-0-flows-194f3c3ade85)

[微软身份平台访问令牌](https://docs.microsoft.com/en-us/azure/active-directory/develop/access-tokens)

[使用 Azure Active Directory 授权(存储)](https://docs.microsoft.com/en-us/rest/api/storageservices/authorize-with-azure-active-directory)

[微软身份平台认证库](https://docs.microsoft.com/en-us/azure/active-directory/develop/reference-v2-libraries)

[如何:使用门户创建可以访问资源的 Azure AD 应用程序和服务主体](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal)