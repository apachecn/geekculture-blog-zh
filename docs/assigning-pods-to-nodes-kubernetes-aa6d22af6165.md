# 将窗格分配给节点| Kubernetes

> 原文：<https://medium.com/geekculture/assigning-pods-to-nodes-kubernetes-aa6d22af6165?source=collection_archive---------4----------------------->

## 深入探讨节点选择器、节点亲缘性和节点名

![](img/3f8202ed8bc9d748f6964be3d660b3bf.png)

Photo by [Maksym Kaharlytskyi](https://unsplash.com/@qwitka?utm_source=medium&utm_medium=referral) on [Unsplash](https://unsplash.com?utm_source=medium&utm_medium=referral)

在 **Kubernetes** 中，由 Kube-scheduler 在节点上调度 pod。Kubernetes 调度程序是一个控制平面进程，它将 pod 分配给节点。根据设置的约束和可用资源，确定每个 pod 的有效位置。然而，可能有这样的情况，我们有一个 pod 需要 GPU 按照我们的预期工作。在这种情况下，我们可能需要控制 pod 的调度，以将 pod 分配给具有可用 GPU 的节点。

有几种方法可以控制调度，推荐的方法是使用**标签选择器**来帮助选择。

为了更进一步，让我们看看如何向节点添加标签并查看节点的标签:

```
**kubectl label nodes <NODE-NAME> <KEY>=<VALUE>** >> kubectl label nodes node01 **gpu=yes**# View the labels of all nodes
>> kubectl get nodes --show-labels# View the labels of a particular node
>> kubectl get nodes **node01** --show-labels | awk '{print $6}'--------------------------------------------------------------------
LABELS
beta.kubernetes.io/arch=amd64,beta.kubernetes.io/os=linux,**gpu=yes**,kubernetes.io/arch=amd64,kubernetes.io/hostname=node01,kubernetes.io/os=linux
```

**移除标签:**

```
**kubectl label node <NODE-NAME> <LABEL>-**
>> kubectl label node node01  gpu-
```

## 节点选择器

`nodeSelector`是向节点分配窗格的最简单方法。我们可以将`nodeSelector`字段添加到 Pod 规范中，并指定我们假设目标节点具有的**节点标签**。Kubernetes 只将 Pod 调度到具有我们在 Pod 规范中指定的每个标签的节点上。如果没有指定标签的可用节点，则 pod 将处于挂起状态。

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
 **nodeSelector:
    gpu: "yes"**
```

## 演练-1

让我们在 pod 定义文件中指定一个在节点标签中不可用的标签。

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
spec:
  containers:
  - name: nginx
    image: nginx
 **nodeSelector:
    ssd: "yes"**
```

使用上述清单创建 pod，并观察 pod 状态:

```
# Create the pod
>> kubectl create -f nginx-pod.yaml# Inspect the pod events
>> kubectl describe pod nginx-pod  | grep -A5 Events--------------------------------------------------------------------
Events:
  Type     Reason            Age    From               Message
  ----     ------            ----   ----               -------
  Warning  FailedScheduling  12m    default-scheduler  0/2 nodes are available: **2 node(s) didn't match Pod's node affinity/selector.** preemption: 0/2 nodes are available: 2 Preemption is not helpful for scheduling.
```

在上面的演示中，我们可以看到由于**标签不匹配**，nginx-pod 处于**挂起状态**。因此，在 pod 定义文件中指定标签时，我们必须格外小心。

## nodeAffinity

**节点关联**的功能类似于`**nodeSelector**` 字段，但更具表现力，允许我们指定软规则。正如我们在前面的阶段中看到的，如果没有指定标签的可用节点，那么 pod 将处于**挂起状态**。使用**节点关联**规则，我们可以克服这个问题。因为**节点关联**支持软/首选规则。以便调度程序仍然调度 Pod，即使它找不到匹配的节点。

**节点亲和类型:**

> `***requiredDuringSchedulingIgnoredDuringExecution***` ***:*** 调度程序无法调度 Pod，除非满足规则。
> 
> `***preferredDuringSchedulingIgnoredDuringExecution***` ***:*** 调度器试图找到符合规则的节点。如果没有匹配的节点，调度程序仍会调度 Pod。

两个**节点亲和**类型之间的常用短语是
`**IgnoredDuringExecution**` *—* 它的意思是将一个 pod 调度成一个节点后。如果确定节点选择的标签被删除。分离舱将照常运行。pod 不会受到影响。

**节点关联**的另一大优势是，我们可以用**操作符**指定一个值列表。假设我们有三种类型的节点可用，每种类型都标记为`**size: large**` **、** `**size: medium**` **、**和`**size: small**`。我们希望将 pod 分配在大型或中型节点上。在这种情况下，如果我们使用`**nodeSelector**`，我们将能够一次指定一个标签`**size: large**`或`**size: medium**`。但是如果我们使用节点关联，我们将能够将`**large**`和`**medium**`指定为一个值列表。然后，pod 将被调度到标记为`**large**`的节点或标记为`**medium**`的节点上。

## requiredduringschedulingignoredduringeexecution

让我们使用清单中指定的节点关联性规则创建一个部署:

```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: webserver
  name: webserver
spec:
  replicas: 3
  selector:
    matchLabels:
      app: webserver
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: webserver
    spec:
 **affinity:
         nodeAffinity:
           requiredDuringSchedulingIgnoredDuringExecution:
             nodeSelectorTerms:
             - matchExpressions:
               - key: size
                 operator: In
                 values:
                 - large
                 - medium** containers:
        - image: nginx
          name: webserver
```

我们可以使用`operator`字段为 Kubernetes 指定一个逻辑操作符，以便在诊断规则时使用。可以用`In`、`NotIn`、`Exists`、`DoesNotExist`、`Gt`、`Lt`。

> ***注:*** *使用* `*NotIn*` *和* `*DoesNotExist*` *运算符我们可以指定反节点亲和规则。*

使用`Exists`操作符创建部署。在`Exists`的情况下，不需要指定任何值，因为它只会检查密钥的存在。

```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: webserver
  name: webserver
spec:
  replicas: 3
  selector:
    matchLabels:
      app: webserver
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: webserver
    spec:
 **affinity:
         nodeAffinity:
           requiredDuringSchedulingIgnoredDuringExecution:
             nodeSelectorTerms:
             - matchExpressions:
               - key: Size
                 operator: Exists** containers:
        - image: nginx
          name: webserver
```

## schedulingignoredduringeexecution 期间首选

在`preferredDuringSchedulingIgnoredDuringExecution` 规则中，我们可以为每个实例指定 1 到 100 之间的`weight`。如果多个节点满足要求，调度器会将该表达式的`weight`值相加。具有最高总数的节点被赋予优先级，并且 pod 被调度到具有最高优先级的节点中。

假设我们有两个节点，一个带有`size: large`标签，另一个带有`size: medium`标签。并且两个节点都符合要求。在这种情况下，调度程序将考虑每个节点的`weight`，并将权重添加到该节点的其他得分中，并将 Pod 调度到最终得分最高的节点上。

```
apiVersion: v1
kind: Pod
metadata:
  name: webserver
spec:
  affinity:
    nodeAffinity:
      preferredDuringSchedulingIgnoredDuringExecution:
       ** - weight: 15**
          preference:
            matchExpressions:
            - key: size
              operator: In
              values:
              - large
       ** - weight: 20**
          preference:
            matchExpressions:
            - key: size
              operator: In
              values:
              - medium
  containers:
  - name: webserver
    image: nginx
```

## 演练 2

部署具有`preferredDuringSchedulingIgnoredDuringExecution`类型的**节点关联**的 pod。和 add 指定节点中不可用作标签的值列表。

```
**# webserver-pod.yaml**apiVersion: v1
kind: Pod
metadata:
  name: webserver
spec:
  affinity:
    nodeAffinity:
      preferredDuringSchedulingIgnoredDuringExecution:
 **- weight: 15**
          preference:
            matchExpressions:
            - key: key
              operator: In
 **values:
              - bar
              - foo**
 **- weight: 20**
          preference:
            matchExpressions:
            - key: key
              operator: In
 **values:
              - foo
              - bar**
  containers:
  - name: webserver
    image: nginx
```

现在，展开吊舱并观察状态:

```
>> kubectl create -f webserver-pod.yaml
>> kubectl get pods webserver--------------------------------------------------------------------
NAME        READY   STATUS    RESTARTS   AGE
webserver   1/1    ** Running**   0          4m4s
```

与[节点选择器](#a652)不同的是，虽然没有匹配的标签可用，但仍然会将 pod 调度到节点中。因为我们使用了`preferredDuringSchedulingIgnoredDuringExecution`类型的**节点关联**。

## 节点名

`nodeName`是将 pod 调度到所需节点的最直接方式。`nodeName`是 Pod 规范中的一个字段。如果在 Pod 规范中指定了`nodeName`字段，调度程序将忽略 Pod，所需节点上的 kubelet 将尝试将 Pod 放置在该节点上。

`nodeName`有一些缺点:

●如果所需的节点不存在，Pod 将不会运行。
●如果目标节点没有容纳 Pod 的资源，Pod 将无法运行。
●云环境中的节点名称并不总是可预测或稳定的。

以下是使用`nodeName`字段的 pod 清单:

```
apiVersion: v1
kind: Pod
metadata:
  name: nginx
spec:
  containers:
  - name: nginx
    image: nginx
 **nodeName: node01**
```

> *如果你觉得这篇文章很有帮助，请* ***别忘了*** *点击* ***拍拍*** *和* ***跟着*** *按钮帮我写更多这样的文章。
> 谢谢🖤*

## 参考

[](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/) [## 将窗格分配给节点

### 您可以约束一个 Pod，使其被限制在特定的节点上运行，或者更喜欢在特定的节点上运行…

kubernetes.io](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/)