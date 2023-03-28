# 使用 Webhook 构建代码的 CloudFormation 示例

> 原文：<https://medium.com/geekculture/cloudformation-example-for-codebuild-with-a-webhook-b442b827e92f?source=collection_archive---------54----------------------->

2021 年 6 月 17 日

作者托马斯

![](img/cb1fe3e7304e1339dcaafcb08cd8563c.png)

Photo by [Grace To](https://unsplash.com/@gigalilac?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) on [Unsplash](https://unsplash.com/s/photos/hook?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在不久前构建一个小项目时，我最终想只使用 CodeBuild 来实现我的 CI/CD。我以前使用 CodePipeline 创建过管道，可以自动从给定的 GitHub 存储库中提取最新的更改并对其进行操作。我知道可以基于 webhooks 配置 CodeBuild 来做类似的事情，但我从未实现过它。设置并不太难，但我确实遇到了一个小问题，我想在这篇文章中强调并解释一下。

以下段落有一些关于 AWS 帐户设置的重要信息，所以请不要跳到模板的底部。

CodeBuild 实例和模板没有什么特别的。IAM 角色告诉 CodeBuild 我能做什么，然后 CodeBuild 项目本身被定义。吸引我的一件事是配置[源凭证](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-codebuild-sourcecredential.html)。

当我最初实现它时，我在与我的 CodeBuild 项目相同的模板中定义了源凭证。每当我想启动另一个实例时(比方说，一个新的环境)，我都会在一个我知道有效的模板上遇到错误。事实证明，每个 AWS 帐户的每个提供者的源凭证必须是唯一的。这意味着每个`ServerType`应该只使用一次`AWS::CodeBuild::SourceCredential`资源类型。不幸的是，我花了将近一个小时四处寻找，试图解决这个问题。阻碍我的错误大概是这样的

```
Failed to call ImportSourceCredentials, reason: Access token with server type GITHUB already exists. Delete its source credential and try again.
```

如果将来有人偶然发现这个问题，要知道，AWS 有不好的错误消息不是你的错。

最后，我想首先给出一个如何创建源凭证的模板，然后给出 CodeBuild 项目本身的模板。这些模板也可以在我的 [AWS CloudFormation 参考 GitHub 资源库](https://github.com/thomasstep/aws-cloudformation-reference/tree/0d6d79f9db33e8ff3f07c9588a822ddff5ad5109/cicd/codebuild)中找到，还有一些其他有用的模板。

首先是源凭证的创建。请记住，每个客户只需记住一次。我目前设置了`Parameters`，假设存在一个名为`codebuild-github-token`的 SSM 参数。您可以随意更改名称，但是我不建议将个人访问令牌直接复制和粘贴到 CloudFormation 参数中。

```
Parameters:
  GitHubAccessToken:
    Type: AWS::SSM::Parameter::Value<String>
    Description: Name of parameter with GitHub access token
    Default: codebuild-github-token
    NoEcho: True

Resources:
  CodeBuildCredentials:
    Type: AWS::CodeBuild::SourceCredential
    Properties:
      ServerType: GITHUB
      AuthType: PERSONAL_ACCESS_TOKEN
      Token: !Ref GitHubAccessToken
```

创建源凭据后，我们可以创建 CodeBuild 项目。

```
Parameters:
  GitHubUrl:
    Type: String
    Description: URL for GitHub repo i.e. https://github.com/username/repository

Resources:
  CodeBuildRole:
    Type: AWS::IAM::Role
    Properties:
      AssumeRolePolicyDocument:
        Version: 2012-10-17
        Statement:
            - Effect: Allow
              Principal:
                  Service:
                    - codebuild.amazonaws.com
              Action:
                - sts:AssumeRole
      Description: !Sub "IAM Role for ${AWS::StackName}"
      Path: '/'
      Policies:
        - PolicyName: root
          PolicyDocument:
            Version: '2012-10-17'
            Statement:
              - Effect: Allow
                Action:
                  - cloudformation:*
                  - codebuild:*
                  - logs:*
                Resource: '*'
  CodeBuild:
    Type: AWS::CodeBuild::Project
    Properties:
      Description: CodeBuild with GitHub webhook
      Triggers:
        BuildType: BUILD
        Webhook: True
        FilterGroups:
        - - Type: EVENT
            Pattern: PUSH,PULL_REQUEST_MERGED
          - Type: HEAD_REF
            Pattern: ^refs/heads/main$
            ExcludeMatchedPattern: false
      ServiceRole: !GetAtt CodeBuildRole.Arn
      Artifacts:
        Type: NO_ARTIFACTS
      Environment:
        Type: LINUX_CONTAINER
        ComputeType: BUILD_GENERAL1_SMALL
        Image: aws/codebuild/standard:4.0
        EnvironmentVariables:
          - Name: MY_ENV_VAR
            Value: something
            Type: PLAINTEXT
      Source:
        Type: GITHUB
        Location: !Ref GitHubUrl
      TimeoutInMinutes: 10
  CodeBuildLogGroup:
    Type: AWS::Logs::LogGroup
    Properties:
      LogGroupName: !Sub "/aws/codebuild/${CodeBuild}"
      RetentionInDays: 7

Outputs:
  ProjectName:
    Value: !Ref CodeBuild
    Description: CodeBuild project name
```

关于这个模板，有一些重要的事情需要注意。首先，IAM 角色需要调整，以允许 CodeBuild 执行特定于您的项目的操作。第二，`FilterGroups`目前被设置为仅触发合并，并直接推送到一个名为`main`的分支，这可能需要根据您的需求进行更改。最后，CodeBuild 实例需要一个名为`buildspec.yml`的文件出现在 repo 的根目录中。如果你不知道什么是`buildspec`，请阅读[这份文档](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html)(它几乎只是一个当代码构建被触发时运行的 bash 脚本)。

*原载于 2021 年 6 月 17 日 https://thomasstep.com**[*。*](https://thomasstep.com/blog/cloudformation-example-for-codebuild-with-a-webhook)*