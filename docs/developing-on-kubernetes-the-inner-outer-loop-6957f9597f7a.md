# 在 Kubernetes 上开发:内部和外部循环

> 原文：<https://medium.com/geekculture/developing-on-kubernetes-the-inner-outer-loop-6957f9597f7a?source=collection_archive---------20----------------------->

> 本文面向在 Kubernetes 或通用系统管理方面有一些经验的开发人员，尽管希望任何人都可以从这些实践中学到一些东西。

kubernetes“K8s”发展成为将服务部署到云的标准方式。理所当然！使用像 K8s 这样的容器编排引擎提供了许多好处，包括但不限于:

*   可伸缩性:*永远不要超出您的基础设施*
*   模块化:*让 K8s 适应你的需求，而不需要修补上游源代码*
*   自我修复:*重启失败的容器*
*   简单服务发现
*   **基础设施抽象的标准化**
*   **部署的清晰原子性**
*   …

K8s 已经发展成为一个行业标准，它允许团队在部署策略方面有一个共同的标准。尽管它没有重新发明软件架构和部署策略的一般概念，但由于 K8s 资源的声明性，这些方法比以往任何时候都更容易掌握。这种标准化至关重要，因为它将个人的专业知识与公司潜在的遗留基础设施强有力地分离，并允许更快速的迭代和创新。

虽然 K8s 在生产中使用时在复杂性和开销方面有缺陷，但这些问题很少超过它提供的好处。因此，我将提供一个我喜欢在 K8s 上开发的开发工作流的简明概述。这包括:

*   (引导项目)
*   内部循环:*局部开发*
*   外环:*部署策略&配置管理*

依我拙见，这些阶段不是互不关联的，而是级联的。因此，每个阶段之间的转换和互操作性至关重要。如果处理得当，生命周期管理轻而易举。此外，我坚信只有在有特定原因的情况下，才应该引入复杂性。

> 尽可能保持简单，但不要更简单。

所有项目文件都可以在 [GitHub](https://github.com/fabius/dev-on-k8s) 上找到。请注意，这些实践是我个人最喜欢的。如果你同意/不同意某些陈述，请随意开始讨论。

# 先决条件

为了以后不中断流程，让我们预先安装所有必要的包。请注意，您可以在每个项目的网站上找到最新的安装说明:

*   码头工人
*   [Kompose](https://github.com/kubernetes/kompose)
*   [迷你库贝](https://github.com/kubernetes/minikube)
*   [库贝克特尔](https://github.com/kubernetes/kubectl)
*   斯卡福德

```
# nix
nix-env -bi docker kompose minikube kubectl skaffold# brew
brew install --cask docker
brew install docker kompose minikube kubernetes-cli skaffold
# start docker desktop manually # openSUSE
sudo zypper in docker kompose minikube kubernetes-client
sudo systemctl enable --now docker
curl -Lo skaffold https://storage.googleapis.com/skaffold/releases/latest/skaffold-linux-amd64 && \
sudo install skaffold /usr/local/bin/# debian
sudo apt install docker.io
sudo systemctl enable --now docker
# kompose
curl -L https://github.com/kubernetes/kompose/releases/download/v1.22.0/kompose-linux-amd64 -o kompose
chmod +x kompose
sudo mv ./kompose /usr/local/bin/kompose
# minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
# kubectl
curl -LO "https://storage.googleapis.com/kubernetes-release/release/**$(**curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt**)**/bin/linux/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
# skaffold
curl -Lo skaffold https://storage.googleapis.com/skaffold/releases/latest/skaffold-linux-amd64 && \
sudo install skaffold /usr/local/bin/# fedora
sudo dnf install moby-engine kompose
sudo systemctl enable --now docker
# minikube
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
# kubectl
curl -LO "https://storage.googleapis.com/kubernetes-release/release/**$(**curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt**)**/bin/linux/amd64/kubectl"
chmod +x ./kubectl
sudo mv ./kubectl /usr/local/bin/kubectl
# skaffold
curl -Lo skaffold https://storage.googleapis.com/skaffold/releases/latest/skaffold-linux-amd64 && \
sudo install skaffold /usr/local/bin/
```

# 启动项目

> 如果你习惯于编写 K8s 资源或者已经有了一个可用的服务，可以完全跳过这一部分。这应该可以简化从 dockerised 设置到部署到 K8s 集群的过渡。

> TL；DR:敏捷思维——不要陷入不同的过程中。专注于要解决的核心问题。最简单的解决方案是正确的。

所以你已经收集了所有的需求并设计了系统的架构。现在到了有趣的部分:编码实际的业务逻辑。但是我们该如何开始呢？我们可以克隆一个最小的支架来匹配我们设计的架构。我们可以使用像`helm create`这样的工具来搭建一些基线 K8s 资源文件，或者只是手工编写那些可爱的 yaml 文件。就我个人而言，我不喜欢在一个全新的项目中引入 K8s，原因有二:

1.  刚开始时，你在本地工作。现在还没有要部署的东西，也没有要编排的东西。那么为什么要在这个时候推出 K8s 呢？
2.  软件开发包括一个**发现和学习**的过程。在实际开发过程中，你经常会发现自己在调整设计的某些部分。让我们不要陷入这个过程，保持事情的灵活性和简单性。

此外，当在您的机器上开始和测试您的软件时，您可以通过完全消除它们(看着您，DNS)来为您自己省去额外的麻烦。就像你习惯的那样运行你的容器。你很可能熟悉`docker run`和`docker compose`。就我个人而言，我喜欢一个最小的`docker-compose.yml`来开始，因为我真的很喜欢它清晰的描述格式。你做你的。

让我们假设这是你的花式服务:

```
// main.go
package mainimport (
        "log"
        "os"
        "time"
)func main() {
        for {
                log.Println("Hello from service " + os.Getenv("NAME"))
                time.Sleep(time.Second)
        }
}
```

可以用这样的`Dockerfile`装箱:

```
# Dockerfile
FROM golang:1.16
COPY main.go .
RUN go build -o myservice main.go
CMD ./myservice
```

最小的`docker-compose.yml`看起来像:

```
# docker-compose.yml
version: "3"
services:
  myservice: # call this anything you like
    build: .
    environment:
      NAME: "kube"
```

这就是运行`docker compose up --build`所需的全部内容，它将构建并运行您的服务。假设您的服务需要一个类似`postgres`的数据库，只需扩展`docker-compose.yml`

```
# docker-compose.yml
version: "3"
services:
  myservice: # call this anything you like
    build: .
    environment:
      NAME: "kube"
    depends_on:
      - db
  db:
    image: postgres:13
    environment:
      POSTGRES_PASSWORD: "postgres"
```

运行`docker compose up --build`将构建您的服务，并在`postgres`启动并运行后运行它。直截了当，不是吗？

一旦您的服务准备好被部署，假设您正在部署到 K8s，您将最终想要放弃`docker compose`并切换到针对 K8s 的开发。幸运的是，我们可以将我们的`docker-compose.yml`转换为它们的 K8s 表示，从而显著平滑过渡:

```
mkdir k8s
kompose convert --out k8s/
rm docker-compose.yml
```

这将在`k8s`目录中创建两个部署资源，让我们看看内部开发循环。但是等等，我们首先需要一个本地 K8s 集群。

# 本地 K8s 集群

选择本地 K8s 集群可能会让人不知所措，因为有很多解决方案。一般来说，您希望尽可能接近生产集群的配置奇偶校验。例如，如果你部署到 EKS，考虑一下 [EKS 快照](https://snapcraft.io/eks)。Minikube 是一个易于使用和可扩展的高保真本地集群，从来没有让我感到惊讶。除非你已经有了不使用 Minikube 的具体理由，否则你不会错的。要使用 Minikube 通过 docker 启动本地 K8s 集群，运行`minikube start --driver=docker`

# 内部循环

根据您的设置，您的本地 K8s 集群可能无法访问您的本地 docker 映像注册表。因此，我们不能使用`kubectl apply -f k8s/`创建我们的资源，直到我们已经构建了我们的服务映像并将其推送到集群的注册中心(显然，我们不想将每个开发映像迭代推送到 DockerHub/GitHub 容器注册中心)。正如您可能注意到的那样，在每次更改时手动触发这个过程是非常繁琐的！幸运的是，有工具可以帮助解决这个问题。最受欢迎的有[斯卡福德](https://github.com/GoogleContainerTools/skaffold)、[花园](https://github.com/garden-io/garden) & [倾斜](https://tilt.dev)。我将展示`skaffold`，因为它是我个人最喜欢的。如果你想更多地了解这些工具是如何脱颖而出的，请查看 Liran Haimovitch 的这个令人敬畏的综合比较。

## 斯卡福德来救援了

我们当前的目录如下所示:

```
$ tree
.
├── Dockerfile
├── k8s
│   ├── db-deployment.yaml
│   └── myservice-deployment.yaml
└── main.go
```

由于它的交互式演练，配置`skaffold`相当简单。`skaffold init`将引导你:

```
$ skaffold init
**? Choose the builder to build image myservice**  [Use arrows to move, type to filter]
**> Docker (Dockerfile)** None (image not built from these sources)
```

将从我们自己的 docker 文件中构建。点击`Enter`确认选择

```
$ skaffold init
**? Choose the builder to build image myservice** Docker (Dockerfile)
**? Choose the builder to build image postgres**  [Use arrows to move, type to filter]
**> None (image not built from these sources)**
```

`postgres`应该是从 DockerHub 拉出来的，所以`None`听起来很合理。点击`Enter`确认选择

```
$ skaffold init
**? Choose the builder to build image myservice** Docker (Dockerfile)
**? Choose the builder to build image postgres** None (image not built from these sources)
apiVersion: skaffold/v2beta17
kind: Config
metadata:
  name: dev-on-k-s
build:
  artifacts:
  - image: myservice
    docker:
      dockerfile: Dockerfile
deploy:
  kubectl:
    manifests:
    - k8s/db-deployment.yaml
    - k8s/myservice-deployment.yaml**? Do you want to write this configuration to skaffold.yaml?** (y/N)
```

`deploy`拿起我们之前生成的两个 K8s 资源。然而，只有`myservice`将建立在使用我们的`Dockerfile`的变化上。这看起来很合理。用`y`确认

**就是这样！**就像这样，我们或任何与我们合作的人都可以使用`skaffold dev`启动我们的服务，而无需任何预先设置。`skaffold`将观察源文件的变化。一旦发生变化，`skaffold`将使用相应的`Dockerfile`重建映像，并触发新的部署。试试吧！从`main.go`改变第 10 行记录的信息，观察`skaffold`立即获得改变。如果你需要一些特定的定制，比如端口转发，你可能会在[ska fold . YAML 参考](https://skaffold.dev/docs/references/yaml/)中找到一个选项。

# 外环

假设我们对我们的服务满意，我们最终希望将它部署到一个动态集群中。最有可能的是，我们希望资源配置在部署环境(如 *local、dev、staging、production* )之间稍微有些不同。这就是健壮的配置管理系统发挥作用的地方。

*注意，我们谈论的是配置管理，而不是通常在集群外部的秘密管理。如果你想升级你的秘密管理，看看* [*金库*](https://www.vaultproject.io) *。*

## 配置管理:Kustomize aka。kubectl 应用-k

> Kustomize 让您自定义原始的，无模板的 YAML 文件的多种用途，留下原来的 YAML 原封不动和可用。Kustomize 使用补丁在现有的标准配置文件上引入特定于环境的变化，而不会干扰它。
> -[https://github.com/kubernetes-sigs/kustomize](https://github.com/kubernetes-sigs/kustomize)

换句话说，Kustomize 将名为`kustomization.yaml`的特定于环境的配置文件中指定的补丁应用于原始 K8s 资源文件。这使得基础设施作为代码原则更加灵活和透明。Kustomize 期望的文件结构如下所示:

```
~/someApp
├── base # the raw, unmodified k8s resource files
│   ├── deployment.yaml
│   ├── kustomization.yaml
│   └── service.yaml
└── overlays # the deployment environment specific patches to apply
    ├── development
    │   ├── cpu_count.yaml
    │   ├── kustomization.yaml
    │   └── replica_count.yaml
    └── production
        ├── cpu_count.yaml
        ├── kustomization.yaml
        └── replica_count.yaml
```

让我们将我们的简单服务 kustomize。目前我们的项目布局如下所示:

```
$ tree
.
├── Dockerfile
├── k8s
│   ├── db-deployment.yaml
│   └── myservice-deployment.yaml
├── main.go
└── skaffold.yaml
```

为了使它符合 Kustomize 的预期结构，我们需要将 K8s 资源文件移动到一个`base`目录中，并在一个`kustomization.yaml`中注册它们

```
mkdir k8s/base
mv k8s/*.yaml k8s/base/
echo "apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomizationresources:
  - db-deployment.yaml
  - myservice-deployment.yaml
" > k8s/base/kustomization.yaml
```

创建特定于环境的补丁就像创建只包含要应用的补丁的`overlays`一样简单。例如，如果我们想要为本地开发环境修补环境变量的值，我们只需要指定 YAML 的这一特定部分，并在引用我们想要应用修补程序的`base`文件的`kustomization.yaml`文件中注册资源:

```
# local environment patches
mkdir -p k8s/overlay/localecho "
**apiVersion: apps/v1
kind: Deployment
metadata:
  name: myservice
spec:
  template:
    spec:
      containers:
        - name: myservice
          env:
            - name: NAME
              value: local** " > k8s/overlay/local/myservice-deployment.yamlecho "
**apiVersion: kustomize.config.k8s.io/v1beta1
kind: Kustomization
patchesStrategicMerge:
  - myservice-deployment.yaml
resources:
  - ../../base**
" > k8s/overlay/local/kustomization.yaml
```

类似地，如果需要的话，我们可以为试运行和生产环境修补 YAML。

## 修复内部循环

现在我们改变了我们的文件结构，我们打破了先前生成的`skaffold.yaml`。谢天谢地，`skaffold`和`kustomize`是可互操作的，所以重新生成`skaffold.yaml`就像再次运行`skaffold init`一样简单。

```
rm skaffold.yaml
skaffold init
```

请注意`skaffold`如何配置其`deploy`阶段以使用 kustomize 的`base`目录，并为`local`环境追加了一个`profile`:

```
# skaffold.yaml
apiVersion: skaffold/v2beta12
kind: Config
metadata:
  name: dev-on-k-s
build:
  artifacts:
  - image: myservice
    docker:
      dockerfile: Dockerfile
**deploy:
  kustomize:
    paths:
    - k8s/base
profiles:
- name: local
  deploy:
    kustomize:
      paths:
      - k8s/overlay/local**
```

`skaffold dev`将默认使用`base`路径。要使用`local`配置，将`local`配置文件指定为 CLI 标志:`skaffold dev -p local`

## 外环:GitOps

> GitOps 的核心思想是拥有一个 Git 存储库，该存储库总是包含生产环境中当前所需的基础设施的声明性描述，以及一个使生产环境与存储库中描述的状态相匹配的自动化过程。如果您想要部署一个新的应用程序或更新一个现有的应用程序，您只需要更新存储库—自动化过程会处理所有其他事情。这就像用巡航控制来管理生产中的应用程序一样。
> -[https://www . gitops . tech](https://www.gitops.tech)

使用 Kustomize 的 YAML 资源配置不同的部署环境允许我们对每个配置进行版本控制。这意味着我们的 CI/CD 管道的 CD 部分包含的代码不会比

```
skaffold run -p <profile>
# you will probably specify a dev/staging/production
# profile that you want to use here respectively
```

它将构建和测试工件，标记它们并将 K8s 清单部署到您的集群中。对于更复杂/细粒度的 CD 管道，考虑不同的`skaffold`调用，如

```
skaffold build --push
skaffold deploy# or to separate the configuration from
# the application's source codeskaffold render --output render.yaml
skaffold apply render.yaml
```

*要了解更多关于 Skaffold 的 CI/CD 工作流程，请查看他们的* [*超棒的 CI/CD 文档*](https://skaffold.dev/docs/workflows/ci-cd/) *。*

# 包扎

为了清理您的本地环境，`minikube delete`将一次性关闭并清理您的 K8s 集群。

重申:当使用正确的工具时，Kubernetes 上的可再现性和应用程序生命周期管理可以轻而易举。Skaffold 和 Kustomize 提供了一个清晰易懂的工作流程。

我希望这对任何人都有帮助。欢迎在评论区联系我，或者在 twitter 上找到我。