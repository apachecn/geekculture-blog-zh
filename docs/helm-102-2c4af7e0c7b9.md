# 舵 102

> 原文：<https://medium.com/geekculture/helm-102-2c4af7e0c7b9?source=collection_archive---------15----------------------->

在设置好 Kubernetes 并为应用程序部署创建了掌舵图之后，您可以在掌舵图中实现一些步骤，使之更加健壮。

# 等待部署成功，然后将工作流标记为完成。

当您运行舵部署(部署滚动)时，您应该确保部署成功。默认情况下，Helm 就像 Kubernetes 一样，不会检查这一点。

然而，Helm 提供了`--wait`标志，等待发布成功部署所有资源。

> 如果设置，将在将发布标记为成功之前等待，直到部署的所有单元、PVC、服务和最小数量的单元都处于就绪状态。它将等待`--timeout`

该标志有多个问题似乎已经解决，但以下两个不便之处仍然存在于等待标志中—更多详细信息可在此处找到:

1.  如果 pod“摆动”(它准备好一会儿，然后失败)，helm 可能会通过工作流，因为副本准备好的时间很少，在我们的情况下这将是一个误报，这通常发生在您的应用程序出现逻辑错误，使其工作一段时间，然后崩溃。
2.  如果您只有 1 个复制副本，并且您已指定 1 个复制副本不可用，则如果 1 个 pod 不可用，则部署在技术上“就绪”。

## 解决办法

第二个问题的解决方案非常简单，要么拥有 1 个以上的副本，要么将最大不可用配置更改为 0。尽管将最大不可用配置设置为 0 会导致执行滚动更新时出现障碍，拥有一个副本也是如此，但解决此问题的最佳方法是最少保留两个副本。

对于第一个问题，您要确保 helm 准确了解 pod 准备就绪的时间，而不仅仅依赖 pod 状态，比如“正在运行”。您可以通过在应用程序中添加探测器来实现这一点。下面是一个样本代码，你可以在这个官方[文档](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-readiness-probes)中进一步阅读关于探针的内容。

```
**kind**: Deployment
**spec**:
  **template**:
    **spec**:
      **containers**:
        **livenessProbe**:
          **httpGet**:
            **path**: /health
            **port**: 9000
          **initialDelaySeconds**: 3
          **periodSeconds**: 3
        **readinessProbe**:
          **httpGet**:
            **path**: /health
            **port**: 9000
          **initialDelaySeconds**: 3
          **periodSeconds**: 3
```

# 如果部署失败，自动回滚

有时你会发出错误的代码，在部署过程中崩溃，你的系统失败，这样的失败状态可以很容易地通过使用下面的 helm `rolback`命令手动回滚到一个稳定的 helm 版本。这将使你的豆荚进入工作状态。

```
helm rollback <RELEASE> [REVISION] [flags]For eg: helm rollback release1 13
```

您可以使用`history`命令查看发布列表，或者使用`status`命令查看当前发布版本。

```
helm status release1 -n default
helm history release1 -n default
```

但是，您可能不希望每次都手动执行，应该集成一个自动化系统，以便在部署失败时优雅地回滚舵释放。Helm 提供了`--atomic`旗帜来做到这一点。

> 如果设置，升级过程将回滚升级失败时所做的更改。如果使用了`--atomic`，则`--wait`标志将被自动设置

# 每次头盔升级/滚动触发部署

当在 k8s 中触发部署时，最新/指定的映像将被提取以用于 pod 部署，但如果最新/指定的映像标记与当前运行的 pod 正在使用的标记相同，则不会触发新的滚动。

这可能会与部署中定义的`Always`拉策略的功能相混淆，因为这是在 [this](https://stackoverflow.com/questions/45905999/kubernetes-image-pull-policy-always-does-not-work) 线程中提出的。

> Kubernetes 不是在等待新版的图片。映像拉取策略指定如何获取映像来运行容器。`*Always*`意味着每次启动一个容器时，它都会尝试获取一个新的版本。要查看更新，您需要删除 Pod(不是部署)——新创建的 Pod 将运行新的映像。
> 
> 没有直接的方法让 Kubernetes 用新的图像自动更新正在运行的容器。

要强制部署，您可以使用下面的命令重新启动`kubectl`部署。

```
kubectl rollout restart deployment/<deployment-name> -n <namespace>
```

然而，`helm`提供了一个更好的方法来解决这个问题，它向您的部署添加了一个随机的字符串/时间戳作为注释，因此它总是会发生变化，并导致部署在您每次运行 helm 升级时滚动。你可以参考这个[螺纹](https://stackoverflow.com/questions/65413154/how-to-force-a-redeploy-with-helm)或者官方掌舵[文档](https://helm.sh/docs/howto/charts_tips_and_tricks/#automatically-roll-deployments)进一步了解。

```
**kind**: Deployment
**spec**:
  **template**:
    **metadata**:
      **annotations**:
        **timestamp**: {{ now | quote }}[...]
```

> 每次调用模板函数都会生成一个唯一的随机字符串。这意味着，如果需要同步多个资源使用的随机字符串，所有相关资源都需要在同一个模板文件中。

# 请等待部署完成，然后再运行另一次升级

如果您试图在可能正在进行另一个部署的同一个 helm 图表上运行 helm upgrade 命令，Helm 将抛出以下错误。

```
Error: UPGRADE FAILED: another operation (install/upgrade/rollback) is in progress
```

您不应该在同一个图表上运行多个升级以保持 pod 的一致性，但同时您也不希望在开始部署之前无所事事地等待部署完成，这听起来像是浪费了很多时间。

要解决这一问题，您可以在工作流中添加一个检查，让部署等待当前完成并自动开始新的升级。不幸的是，helm 不提供任何等待部署完成的命令；但是 Kubernetes 知道。因此，虽然你不能等待头盔展示完成，你可以等待特定的部署完成，这是头盔部署的一个子部分。

如果部署成功，将输出`kubectl rollout status`命令，或者继续提供更新，直到部署成功。下面是该命令如何工作的几个例子。

```
$ kubectl rollout status deploy stage-backend deployment "stage-backend" successfully rolled out------------------$ kubectl rollout status deploy stage-backendWaiting for deployment "stage-backend" rollout to finish: 1 old replicas are pending termination...
Waiting for deployment "stage-backend" rollout to finish: 1 old replicas are pending termination...
Waiting for deployment "stage-backend" rollout to finish: 1 old replicas are pending termination...
Waiting for deployment "stage-backend" rollout to finish: 3 of 4 updated replicas are available...
deployment "stage-backend" successfully rolled out--------------------$ kubectl rollout status deploy stage-backend Waiting for deployment "stage-backend" rollout to finish: 2 out of 4 new replicas have been updated...
error: deployment "stage-backend" exceeded its progress deadline
```

这种方法有时会变得不可靠，比如如果您在单个 helm 图表下有多个部署，或者当代码崩溃并且 helm 开始回滚时——从技术上讲，部署滚动尚未完成，但是`kubectl rollout`会退出，说它在回滚开始之前失败了。但是，可以通过创建一个脚本，使用`helm status`子命令持续轮询图表部署的状态来解决这个问题。

```
$ helm status release1 | grep STATUS | cut -d: -f2
deployed
```

也就是说，这个问题的一个更简单的解决方案是不要让升级作业的多个实例在您的 CI/CD 管道中运行。例如，如果您使用 Github 操作，您可以在工作流中使用`concurrency`关键字，以确保升级工作流仅在没有其他工作流运行时运行，如果有，它会将当前工作流发送到挂起状态，这将在前一个工作流完成后执行。您还可以在作业级别指定`concurrency`。更多信息请参见`[jobs.<job_id>.concurrency](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idconcurrency)`。

你可以从这个官方的 Github [博客](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#concurrency)中了解更多关于这个特性的信息。