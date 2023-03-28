# 为机密管理器机密创建白名单

> 原文：<https://medium.com/geekculture/creating-a-whitelist-for-a-secrets-manager-secret-e4860672cab9?source=collection_archive---------55----------------------->

2021 年 5 月 24 日

作者托马斯

![](img/227f334a97b13dce786c975b93e2c808.png)

Photo by [Silas Köhler](https://unsplash.com/@silas_crioco?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/key?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

不久前，我试图阻止除了少数 IAM 角色和用户之外的任何人访问 AWS Secrets Manager 中的秘密。这看起来似乎是一个简单的任务，但也有一些挑战。我将包含一个模板的片段来检查不同的部分，但如果你宁愿只是抓取整个模板的副本，我已经将它添加到我的[AWS-cloudformation-reference GitHub repo](https://github.com/thomasstep/aws-cloudformation-reference/blob/master/secretsmanager/whitelisted-secret.yml)中，以及我作为参考点创建的其他 cloud formation 模板。以下是附加到机密的资源策略版本，它允许访问特定的 IAM 角色和用户。

```
Parameters:
  WhitelistedRoleIds:
    Type: CommaDelimitedList
    Description: >
      IDs (AROA*, or AIDA*) to whitelist access for on the created secret.
      Find the Role ID by running aws iam get-role --role-name ROLE-NAME.
      Remember to append :* to the end of the Role IDs.
      Find the User ID by running aws iam get-user --user-name USER-NAME.
      User IDs do not need :* appended to the end.

Resources:
  ...
  SecretResourcePolicy:
    Type: 'AWS::SecretsManager::ResourcePolicy'
    Properties:
        SecretId: !Ref MySecret
        ResourcePolicy:
          Version: 2012-10-17
          Statement:
            - Resource: !Ref MySecret
              Action: 'secretsmanager:GetSecretValue'
              Effect: Deny
              Principal: '*'
              Condition:
                StringNotLike:
                  aws:userId: !Ref WhitelistedRoleIds
```

有一些值得指出的细微差别是我在弄清楚这个配置时偶然发现的。

第一，我想用资源政策打基础。当我遇到这个问题时，我已经知道(尽管没有太多的实践经验)，一些 AWS 资源允许我们将资源策略附加到它们上面。其他一些资源包括 S3 和 Lambda(关于一个庞大的列表[查看本页](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_aws-services-that-work-with-iam.html)的“基于资源的政策”一栏)。

资源策略允许我们[基于一些关键属性创建访问和拒绝的捷径](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html#policy-eval-denyallow)，这些属性在资源的大多数 API 调用中都可用。这意味着，即使 IAM 角色明确允许访问某个资源，资源策略也可以明确拒绝该角色的访问，从而拒绝承担该角色的资源。反之亦然，如果资源策略明确授予访问权限，则拒绝访问资源的 IAM 角色将被覆盖。

我试图在脑海中简化这一点，认为无论什么规则都是最接近资源获胜的。IAM 角色可以是一般的东西，但是资源策略直接附加到资源。这意味着资源策略上的规则每次都胜过 IAM 角色。

在我们的资源策略知识的基础上，我们开始进入这个特定的 Secrets Manager 资源策略的具体细节。由于我们希望创建一个白名单，并将所有其他 IAM 资源列入黑名单，因此我们需要明确拒绝除白名单资源之外的所有资源。不幸的是，我们不能将其简化为在一个语句中对所有内容进行显式拒绝，然后在另一个语句中给出显式允许。规则对自身进行优先排序的方式会让“否定一切”优先。

现在我们已经了解了什么是资源策略，下一步是了解在 API 调用 Secret 时哪些类型的属性是可用的，根据这些属性我们可以授予或拒绝访问权限。我尝试使用的第一个也是最明显的资源策略配置类型是`NotPrincipal`。这类似于前述资源策略[中的`Principal`，除了它做相反的事情](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_elements_notprincipal.html#specifying-notprincipal)。我开始尝试完成同样的目标，但是使用了`NotPrincipal`，我希望它看起来像下面这样。

```
- Resource: !Ref MySecret
  Action: 'secretsmanager:GetSecretValue'
  Effect: Deny
  NotPrincipal:
    AWS: !Ref WhitelistedPrincipals
```

不幸的是，当一个 API 调用来自一个假定的 IAM 角色时，它看起来不再像`arn:aws:sts::AWS_ACCOUNT_ID:assumed-role/ROLE_NAME/SESSION_NAME`，它也被称为假定的角色 ARN。由于`Principal`由 IAM 角色的 ARN 和假定角色 ARN 组成，IAM 将拒绝对资源的访问，因为`Principal`并不完全匹配。

`aws:userId`传入。经过一番搜索，我最终登陆了一个页面[，上面列出了 IAM 策略条件关键字](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_condition-keys.html#condition-keys-userid)。这是第一部分。第二部分是一篇关于限制使用 S3 水桶的博文，类似于我想用我的秘密做的事情。这篇文章解释说，已经假定的 IAM 角色将总是在`aws:userId`变量中传递相同的角色 ID 以及会话名称。我喜欢把这个独特的角色 ID 看作是 IAM 角色 ARN 和虚拟角色 ARN 的混合体。因为它是一个单一变量，所以我们可以在资源策略中基于它授予访问权限。

如果有人好奇，`AROA*`和`AIDA*`是由 AWS 预定义的[唯一前缀。我记得在浏览一些不相关的日志时注意到了这种模式，并想知道它们是否都有意义。事实证明确实如此。](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_identifiers.html#identifiers-unique-ids)

使用`aws:userId`变量允许我们将 IAM 用户和角色列入白名单。IAM 角色需要像`AROAJQABLZS4A3QDU576Q:*`一样被列入白名单，因为会话名被附加到角色 ID 的末尾。(在进行 API 调用之前，必须首先假定角色。)一个 IAM 用户需要像`AIDAJQABLZS4A3QDU576Q`一样被列入白名单，但不附加`:*`。要找到那些看起来很时髦的 id，运行`aws iam get-role --role-name ROLE_NAME`或`aws iam get-user --user-name USER_NAME`。

我不得不走了好几个兔子洞才弄明白这一点，这就是为什么它需要一个故事而不是一个简单的片段。对于任何可能遇到这种情况并需要更多帮助的人，请随时联系我，我会尽我所能！

*原载于 2021 年 5 月 24 日 https://thomasstep.com**[*。*](https://thomasstep.com/blog/creating-a-whitelist-for-secrets-manager-secret)*