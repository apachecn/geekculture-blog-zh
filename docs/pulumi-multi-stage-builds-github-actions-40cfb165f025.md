# 如何让 Pulumi、GitHub 动作和多阶段构建协同工作

> 原文：<https://medium.com/geekculture/pulumi-multi-stage-builds-github-actions-40cfb165f025?source=collection_archive---------18----------------------->

![](img/4b74ae87e6bbef69f09ac81ae1d4e630.png)

Photo by [Marc-Olivier Jodoin](https://unsplash.com/@marcojodoin?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

**TL；DR:** 使用`buildx`和一些附加选项:

```
const image = repo.buildAndPushImage({
  target: 'final_stage',
  cacheFrom: { stages: ['cached_stage_1', 'cached_stage_2'] },
  extraOptions: ['--load'],
  args: { BUILDKIT_INLINE_CACHE: '1' },
})
```

**目录**

*   [为什么是普鲁米？](#5f30)
*   [多阶段构建](#872a)
*   [添加 GitHub 动作](#eb7a)
*   [添加普鲁米](#613d)

最近，当我们将一个堆栈从 Heroku 迁移到 AWS 时，我有机会在 Nayya 开始使用 Pulumi。Pulumi 是一个基础设施即代码(IaC)平台，我对它感兴趣已经有一段时间了，它非常适合我们的基础设施/部署需求。

# 为什么是普鲁米？

与我使用过的其他 IaC 工具相比，我更喜欢 Pulumi 的一点是:开发人员可以用熟悉的编程语言(如 Typescript 或 Python)来定义基础设施，而不是用特定于领域的语言或标记来编写。

下面是在 **Terraform** 中创建几个 IAM 用户的样子:

```
variable "user_names" {
  description = "Create IAM users with these names"
  type        = list(string)
  default     = ["tariq", "ahmir", "kamal"]
}resource "aws_iam_user" "users" {
  for_each = toset(var.user_names)
  name     = each.value
}# First user = aws_iam_user.users[0]
```

下面是它在**普鲁米**中的样子，使用的是打字稿:

```
const config = new pulumi.Config()
const usernames = config.requireObject<string[]>('usernames')const users = usernames.map(name => new aws.iam.User(name))// First user = users[0]
```

Pulumi/Typescript 对我来说不那么冗长，更直观，我喜欢能够在 IaC 中使用常见的编程语言。在 Nayya，我们使用了 Ruby、Typescript/JS/Node、Python 和其他语言的组合，对 IaC 使用 Typescript 感觉很合适。🚀

# 我们如何保持 Docker 构建速度快，图像小？

一个很好的策略是[多阶段构建](https://docs.docker.com/develop/develop-images/multistage-build/)。在本地构建多阶段 Dockerfile 时，第一次构建将为构建的每个阶段生成一个映像，这些映像将用于缓存这些层以供将来构建使用。

这样，我们可以使用一个阶段来安装我们的依赖项，另一个阶段来构建资产(Webpack)，最后一个阶段来复制前面阶段不需要的所有文件。这样，如果自上次构建以来依赖项没有改变，依赖项阶段将被缓存。如果网络打包的文件没有改变，构建资产阶段将被缓存。如果没有文件发生更改，最终阶段也将被缓存。这有助于我们快速构建！

此外，我们可以使用一个全功能的基础映像来构建我们的应用程序，然后将生成的工件复制到一个更小的基础映像中进行生产。这里有一个例子`Dockerfile`:

```
# Large base image with build tools, caches our dependencies:
FROM node as deps
RUN yarn install# Build stage:
FROM deps as build
RUN yarn build# Small base image, copy artifacts:
FROM node:slim as production
COPY --from=build ./node_modules .
COPY --from=build ./build .
```

**⚠️问题⚠️** 不幸的是，当我们使用云 CI 平台构建我们的映像时，舞台映像不会在本地缓存，因为 runner 每次都是从头开始；每次构建开始时都没有本地图像缓存！

# 解决仅使用 GitHub 动作时的问题

如果您在 GitHub 动作上构建 Docker 图像而不使用 Pulumi，有一个简单的解决方法:Docker 带缓存构建动作[。我使用 ECR 作为我的注册表，所以在 GitHub Actions 工作流文件中，它看起来像这样:](https://github.com/whoan/docker-build-with-cache-action)

```
jobs:
  job_name:
    steps:
      (...) - name: Configure AWS credentials
      uses: aws-actions/configure-aws-credentials@v1
      with:
        aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
        aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
        aws-region: ${{ env.AWS_REGION }} - name: Login to Amazon ECR
      id: login-ecr
      uses: aws-actions/amazon-ecr-login@v1 - name: Build and push (with cache)
      id: build-and-push-with-cache
      uses: **whoan/docker-build-with-cache-action**@v5
      with:
        registry: ${{ steps.login-ecr.outputs.registry }}
        image_name: image_name_here
        image_tag: ${{ github.sha }}-production
```

这将创建 ECR 存储库`image_name_here`和`image_name_here-stages`(如果它们还不存在的话)。前者将用于上传最终的舞台构建图像，后者将用于上传舞台图像。`whoan/docker-build-with-cache-action@v5`动作首先提取现有的舞台图像和最终图像，然后使用这些图像运行 docker build 进行缓存，然后将最终的最终图像和舞台图像推回到这些存储库中。

**但是，如果我们使用 Pulumi，Docker 构建应该在 Pulumi 中定义，所以我们不能使用上面的方法。**

# ✨解决了在 GitHub Actions ✨上使用 Pulumi 的问题

方便的是，Pulumi 有一个 [Github 动作](https://github.com/pulumi/actions)，处理 CLI 的安装和运行`pulumi up`。我们将使用它在 GitHub 动作工作流中运行 Pulumi。

在 Pulumi 中，我们可以创建 ECR 存储库和推送到该存储库的图像。当我第一次尝试这种方法时，我遵循文档并得出了这个结论:

```
// DO NOT USE:const repo = new awsx.ecr.Repository('app_name-stack_name')const image = repo.buildAndPushImage({
  target: 'production',
  cacheFrom: { stages: ['deps', 'build', 'production'] },
})
```

当我在本地运行`pulumi up`时，这非常有效！它拉下`image_name:deps`、`image_name:build`和`image_name:production`，用缓存构建图像，然后推送新的图像。注意:这种方法不像`whoan/docker-build-with-cache-action`那样为阶段创建单独的存储库，而是为不同的阶段使用不同的标签。

**不幸的是，当我在 GitHub Actions 工作流中运行 Pulumi 堆栈更新时，我遇到了几个问题:**

1.  在 docker 构建之前，已经从 ECS 中正确提取了阶段映像，但是这些映像没有用于缓存，因此即使每个阶段都应该被缓存，也要重新构建。
2.  这些阶段甚至没有在本地正确缓存，所以对于我的 3 个阶段，它将再次递归构建每个依赖阶段，而没有缓存！于是它建造了`deps`，然后作为`build`的一部分再次建造了`deps`，然后建造了`build`，然后作为`production`的一部分再次建造了`deps`和`build`，然后建造了`production`。一个完全缓存的构建应该不到一分钟——正好是提取所有层并确认每一步都可以缓存的时间——这花费了 **30 分钟**！😱

根据 Pulumi 社区 Slack 的一些建议，我尝试用 docker/setup-buildx-action 将 Buildx 添加到我的 GitHub Action 构建环境中。我选择使用`docker`构建驱动程序，而不是默认的`buildx`驱动程序`docker-container`，以便更好地与 Pulumi 互操作。该操作如下所示:

```
 - uses: docker/setup-buildx-action@v1
        with:
          install: true
          driver: docker
          buildkitd-flags: --debug
```

这是朝着正确方向迈出的一步，但是我需要在 Pulumi 的`docker build`调用中添加`--load`选项。这是必要的，因为默认情况下，`buildx`不会将构建的图像存储在`docker images`本地缓存中:

```
const image = repo.buildAndPushImage({
  target: 'production',
  cacheFrom: { stages: ['deps', 'build', 'production'] },
  extraOptions: ['--load'],
})
```

修复了问题 2——疯狂的递归构建不再是一个问题。然而，缓存仍然没有正常工作，因为`docker`构建驱动程序需要`BUILDKIT_INLINE_CACHE=1`被设置为正确内联每个阶段的缓存。添加了这个选项后，构建缓存就像预期的那样工作了

我还从`cacheFrom.stages`数组中删除了目标阶段，因为这给流程增加了一个不必要的步骤。

最后，我添加了一个检查来确保`buildx`被启用，这样本地运行`pulumi up`就不会失败:

```
import { spawnSync, SpawnSyncReturns } from 'child_process'const cmd = (input: string): SpawnSyncReturns<string> => {
  const [command, ...args] = input.split(' ').filter((str) => str)
  const result = spawnSync(command, args, {
    encoding: 'utf-8',
  }) as unknown as SpawnSyncReturns<string> if (result.status !== 0) {
    throw new Error(result.stderr)
  } return result
}export const checkForBuildx = () => {
  if (!cmd('docker build --help').stdout.includes('buildx')) {
    throw new Error(
      'Buildx must be enabled! Please run: docker buildx install'
    )
  }
}
```

这是最终的工作代码:

```
checkForBuildx()const image = repo.buildAndPushImage({
  target: 'production',
  cacheFrom: { stages: ['deps', 'build'] },
  extraOptions: ['--load'],
  args: { BUILDKIT_INLINE_CACHE: '1' },
})
```

感谢阅读！

想加入一个快速发展的充满激情的工程师团队吗？ [Nayya](https://nayya.com) 踏上了让数百万家庭做出更明智的健康、保险和财务决策的旅程——来吧[加入我们的行列](https://nayya.com/careers)！