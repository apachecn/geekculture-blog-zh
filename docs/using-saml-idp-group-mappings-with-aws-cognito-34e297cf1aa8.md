# 通过 AWS Cognito 使用 SAML IdP 组映射

> 原文：<https://medium.com/geekculture/using-saml-idp-group-mappings-with-aws-cognito-34e297cf1aa8?source=collection_archive---------2----------------------->

AWS Cognito 是一种流行的托管身份验证服务，它为集成的 SAML 2.0 兼容身份提供者(IDP)提供支持，如 Azure Active Directory、Okta、Auth0、OneLogin 等。

Cognito 的一个用例是充当身份提供者和后端 web 应用程序之间的中间件或代理层。开发人员不用直接在应用程序中实现对 SAML 的支持(并处理适当的安全配置和各种标准)，而是可以使用 Cognito 来完成繁重的工作。