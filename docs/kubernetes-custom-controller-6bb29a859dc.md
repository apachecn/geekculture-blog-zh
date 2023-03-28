# 创建 Kubernetes 控制器

> 原文：<https://medium.com/geekculture/kubernetes-custom-controller-6bb29a859dc?source=collection_archive---------13----------------------->

![](img/a5057f44ac69a4921055dbc9863b3f06.png)

Kubernetes 被广泛用于构建云原生应用，并在云环境中工作。由于 kubernetes 是开源的，健壮工具背后用于编排正在运行的微服务容器的架构可以在您自己的项目中扩展。

使用云或构建自己的云的行业使用 kubernetes。我们可以通过查看所有提供 kubernetes 的云提供商服务来了解 kubernetes 的重要性，这些服务旨在创建供客户使用的多集群环境。我不会深入讨论什么是 kubernetes 以及它是如何工作的，请阅读 kubernetes 在 [kubernetes.io](http://kubernetes.io) 上的文档以了解更多关于它的使用。

[](https://kubernetes.io/docs/home/) [## Kubernetes 文档

### 2021 Kubernetes 作者|根据 4.0 分发的文档版权所有 2021 Linux 基金会。所有…

kubernetes.io](https://kubernetes.io/docs/home/) 

kubernetes 提供了一种方法来构建您自己的自定义 Kubernetes 资源，称为 Kubernetes 自定义资源，以便根据您的需求扩展其功能。举例来说，您可以使用 kubernetes 模式创建自己的自定义对象。

Kubernetes 作为云开发的一部分已经有很长时间了，并且有开源库可以将其与我们的项目集成。Kubernetes `operators`提供了管理您的 Kubernetes 资源的最佳方式，并且易于实施。引用自 [kubernetes.io](http://kubernetes.io) 博客:-

> [操作符](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/)被证明是在 Kubernetes 中运行有状态分布式应用程序的优秀解决方案。像 [Operator SDK](https://sdk.operatorframework.io/) 这样的开源工具提供了构建可靠且可维护的操作符的方法，使得扩展 Kubernetes 和实现定制调度更加容易

控制器是用来管理 kubernetes 资源的控制回路。

*在 Kubernetes 中，控制器是控制循环，它监视您的* [*集群*](https://kubernetes.io/docs/reference/glossary/?all=true#term-cluster) *的状态，然后在需要的地方做出或请求改变。每个控制器都试图将当前的群集状态移至更接近所需的状态。*

让我们首先创建一个控制器来监视 kubernetes pod 中的更新，然后向它添加一个标签。为了编写控制器，我们将使用 operator sdk，它将创建一个引导代码。如果您在本地环境中安装了 minikube 或任何其他本地工具(如 k3d ),我们可以使用以下工具开始创建 pod

```
$ kubectl run --image=nginx my-nginx
```

安装 operator-sdk 以创建引导控制器:-

 [## 装置

### 安装 Operator SDK CLI 如果您使用的是 Homebrew，可以使用以下命令安装 SDK CLI 工具…

sdk.operatorframework.io](https://sdk.operatorframework.io/docs/installation/) 

安装完成后，我们可以验证 operator-sdk 的版本，以确保它与 kubernetes 的更新版本相匹配。

```
$ operator-sdk version
```

下一步将是创建一个目录，初始化操作员，该操作员将处理 pod 资源的控制器，最后我们创建一个设置为 pod 的控制器。

```
$ mkdir custom-controller && cd custom-controller
$ operator-sdk init --domain=domain.com --repo=github.com/<user>/custom-controller
$ operator-sdk create api --group=core --version=v1 --kind=Pod --controller=true --resource=false
```

如果您在尝试初始化操作符时遇到 go 版本错误，请跳过初始化操作符命令中的 go 版本检查，但它仍然适用于 go 和 kubernetes 的更新版本。

```
$ operator-sdk init --domain=domain.com --repo=github.com/<user>/custom-controller --skip-go-version-check
```

这将在自定义控制器回购中生成一个`controller/pod_controller.go`。在该文件中，我们需要更新我们的逻辑来观察注释，并使用控制器向 pod 添加一个标签。

```
**func** (r *PodReconciler) Reconcile(ctx context.Context, req ctrl.Request) (ctrl.Result, **error**) {
    _ = r.Log.WithValues("pod", req.NamespacedName)

    *// your logic here* 
    **return** ctrl.Result{}, **nil**
}
```

对 pod 的任何操作都将调用 Reconcile 方法，pod 的名称和命名空间可以从`ctrl.Request`结构中提取。

完成代码以观察 pod 内的注释并向其添加标签:-

现在运行本地 kubernetes 集群设置，并使用`make run`命令从终端运行代码，确保您已经安装了 minikube 来在本地环境中运行集群。

```
2021-10-31T13:30:04.771+0530 INFO controller-runtime.manager.controller.pod Starting EventSource {"reconciler group": "", "reconciler kind": "Pod", "source": "kind source: /, Kind="}
2021-10-31T13:30:04.771+0530 INFO controller-runtime.manager.controller.pod Starting Controller {"reconciler group": "", "reconciler kind": "Pod"}
2021-10-31T13:30:04.872+0530 INFO controller-runtime.manager.controller.pod Starting workers {"reconciler group": "", "reconciler kind": "Pod", "worker count": 1}
```

在另一个终端中用 image nginx 创建一个 pod，并添加注释，以查看在运行 go server 的另一个终端上使用`make run` :-

```
$ kubectl run --image=nginx my-nginx
NAME       READY   STATUS    RESTARTS   AGE   LABELS
my-nginx   1/1     Running   0          18s   run=my-nginx
```

pod 运行后，将注释添加到正在运行的 pod，这将触发一个事件，导致添加 pod 的名称作为标签，如上面的`pod_controller.go`代码中所定义。

```
$ kubectl annotate pod my-nginx custom/add-pod-name-label=true
pod/my-nginx annotated
```

go 控制器运行将显示触发的事件`adding label`，我们可以使用`kubectl`显示标签命令查看添加到 pod 的标签:-

```
$ kubectl get pods my-nginx --show-labels
NAME       READY   STATUS    RESTARTS   AGE   LABELS
my-nginx   1/1     Running   1          7d    custom/pod-name=my-nginx,run=my-nginx
```

总结文章，`operator-sdk`是真正方便的 sdk 来创建 kubernetes 控制器。而我们只需要担心更新控制器的`Reconcile`方法内部的逻辑。完整代码可在[https://github.com/Himanshuxone/label-operator](https://github.com/Himanshuxone/label-operator)获得。

关注[https://golang-geek.medium.com/](https://golang-geek.medium.com/)获取更多关于云计算和其他技术的文章。如有任何问题，请在评论中提及，或给*himanshusdec8@gmail.com*写电子邮件，获取关于云的信息。

联系人:*himanshusdec8@gmail.com*