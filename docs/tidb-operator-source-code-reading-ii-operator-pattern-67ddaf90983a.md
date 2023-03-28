# TiDB 运算符源代码阅读(二):运算符模式

> 原文：<https://medium.com/geekculture/tidb-operator-source-code-reading-ii-operator-pattern-67ddaf90983a?source=collection_archive---------55----------------------->

![](img/ab25b90673b0a6f82b5777bcfb0fe5aa.png)

**作者:** [陈一文](https://github.com/handlerww)(TiDB 运营负责人)

**Transcreator:** [黄然](https://github.com/ran-huang)；编辑:汤姆·万德

在我的上一篇文章中，我介绍了 TiDB Operator 的架构和它的功能。但是 TiDB 操作符代码是如何运行的呢？TiDB 运营商如何管理 TiDB 集群中每个组件的生命周期？

在这篇文章中，我将介绍 Kubernetes 的 Operator 模式，以及它是如何在 TiDB Operator 中实现的。更具体地说，我们将浏览 TiDB 运营商的主要控制循环，从其入口点到生命周期管理的触发点。

# 从控制者到操作员

因为 TiDB 运算符是从[kube-controller-manager](https://kubernetes.io/docs/reference/command-line-tools-reference/kube-controller-manager/)中学习的，所以理解`kube-controller-manager`的设计有助于你更好地理解 TiDB 运算符的内部逻辑。

在 Kubernetes 中，各种[控制器](https://kubernetes.io/docs/concepts/architecture/controller/)管理资源的生命周期，比如名称空间、节点、部署和状态集。控制器监视群集资源的当前状态，将其与所需状态进行比较，并将群集移向所需状态。Kubernetes 的内置控制器在`kube-controller-manager`内部运行，并由它管理。

为了允许用户定制资源管理，Kubernetes 提出了[操作者模式](https://kubernetes.io/docs/concepts/extend-kubernetes/operator/)。用户可以通过定义[自定义资源定义](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/#customresourcedefinitions) (CRDs)对象来创建自己的[自定义资源](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/) (CRs)，并使用自定义控制器来监视相应 CRs 的状态，完成相关的管理任务。Operator 模式允许用户在不修改代码的情况下扩展 Kubernetes 的行为。

# TiDB 控制器管理器如何工作

正如我在上一篇文章中所介绍的，TiDB Operator 有一个核心组件`tidb-controller-manager`，它运行一组定制控制器来管理 TiDB 的 CRD。

## 入口点

从`cmd/controller-manager/main.go`开始，`tidb-controller-manager`首先加载 kubeconfig 来访问 kube-apiserver。然后，它使用一系列的`NewController`函数来加载每个控制器的 init 函数。

```
controllers := []Controller{
    tidbcluster.NewController(deps),
    dmcluster.NewController(deps),
    backup.NewController(deps),
    restore.NewController(deps),
    backupschedule.NewController(deps),
    tidbinitializer.NewController(deps),
    tidbmonitor.NewController(deps),
}
```

在 init 函数的执行过程中，`tidb-controller-manager`初始化一组通知器，这些通知器与 kube-apiserver 交互以获取 CRs 和相关资源的变化。以`TidbCluster`为例，在`NewController`函数中，`tidb-controller-manager`初始化 Informer 对象:

```
tidbClusterInformer.Informer().AddEventHandler(cache.ResourceEventHandlerFuncs{
        AddFunc: c.enqueueTidbCluster,
        UpdateFunc: func(old, cur interface{}) {
            c.enqueueTidbCluster(cur)
        },
        DeleteFunc: c.enqueueTidbCluster,
    })
statefulsetInformer.Informer().AddEventHandler(cache.ResourceEventHandlerFuncs{
        AddFunc: c.addStatefulSet,
        UpdateFunc: func(old, cur interface{}) {
            c.updateStatefulSet(old, cur)
        },
        DeleteFunc: c.deleteStatefulSet,
    })
```

处理`add`、`update`和`delete`事件的 EventHandlers 被注册给通知者。这些事件处理程序处理事件，并将相关的 CR 键添加到队列中。

## 控制器的内部逻辑

初始化后，`tidb-controller-manager`启动 InformerFactory 并等待缓存同步完成:

```
informerFactories := []InformerFactory{
            deps.InformerFactory,
            deps.KubeInformerFactory,
            deps.LabelFilterKubeInformerFactory,
        }
        for _, f := range informerFactories {
            f.Start(ctx.Done())
            for v, synced := range f.WaitForCacheSync(wait.NeverStop) {
                if !synced {
                    klog.Fatalf("error syncing informer for %v", v)
                }
            }
        }
```

接下来，`tidb-controller-manager`调用每个控制器的运行函数，循环执行控制器的内部逻辑:

```
// Start syncLoop for all controllers.
for _,controller := range controllers {
    c := controller
    go wait.Forever(func() { c.Run(cliCfg.Workers,ctx.Done()) },cliCfg.WaitDuration)
}
```

再次以`TidbCluster`控制器为例。Run 函数启动工作队列:

```
// Run runs the tidbcluster controller.
func (c *Controller) Run(workers int, stopCh <-chan struct{}) {
    defer utilruntime.HandleCrash()
    defer c.queue.ShutDown() klog.Info("Starting tidbcluster controller")
    defer klog.Info("Shutting down tidbcluster controller") for i := 0; i < workers; i++ {
        go wait.Until(c.worker, time.Second, stopCh)
    } <-stopCh
}
```

工人调用`processNextWorkItem`函数，将项目出队，并调用`sync`函数来同步 CR:

```
// worker runs a worker goroutine that invokes processNextWorkItem until the controller's queue is closed.
func (c *Controller) worker() {
    for c.processNextWorkItem() {
    }
}// processNextWorkItem dequeues items, processes them, and marks them done. It enforces that the syncHandler is never
// invoked concurrently with the same key.
func (c *Controller) processNextWorkItem() bool {
    key, quit := c.queue.Get()
    if quit {
        return false
    }
    defer c.queue.Done(key)
    if err := c.sync(key.(string)); err != nil {
        if perrors.Find(err, controller.IsRequeueError) != nil {
            klog.Infof("TidbCluster: %v, still need sync: %v, requeuing", key.(string), err)
        } else {
            utilruntime.HandleError(fmt.Errorf("TidbCluster: %v, sync failed %v, requeuing", key.(string), err))
        }
        c.queue.AddRateLimited(key)
    } else {
        c.queue.Forget(key)
    }
    return true
}
```

基于该键，`sync`函数获得相应的 CR 对象(例如，`TidbCluster`对象)并将其同步:

```
// sync syncs the given tidbcluster.
func (c *Controller) sync(key string) error {
    startTime := time.Now()
    defer func() {
        klog.V(4).Infof("Finished syncing TidbCluster %q (%v)", key, time.Since(startTime))
    }() ns, name, err := cache.SplitMetaNamespaceKey(key)
    if err != nil {
        return err
    }
    tc, err := c.deps.TiDBClusterLister.TidbClusters(ns).Get(name)
    if errors.IsNotFound(err) {
        klog.Infof("TidbCluster has been deleted %v", key)
        return nil
    }
    if err != nil {
        return err
    } return c.syncTidbCluster(tc.DeepCopy())
}func (c *Controller) syncTidbCluster(tc *v1alpha1.TidbCluster) error {
    return c.control.UpdateTidbCluster(tc)
}
```

`syncTidbCluster`函数调用`updateTidbCluster`函数，后者进一步调用一系列组件`sync`函数，完成整个 TiDB 集群的管理。

在`pkg/controller/tidbcluster/tidb_cluster_control.go`中，您可以查看`updateTidbCluster`功能的实现，这里有描述每个`sync`功能所执行的生命周期管理的注释。通过这些注释，您可以了解协调每个组件需要哪些操作。例如，在布局驱动器(PD)中:

```
// To make the current state of the pd cluster match the desired state:
//   - create or update the pd service
//   - create or update the pd headless service
//   - create the pd statefulset if it does not exist
//   - sync pd cluster status from pd to TidbCluster object
//   - upgrade the pd cluster
//   - scale out/in the pd cluster
//   - failover the pd cluster
if err := c.pdMemberManager.Sync(tc); err != nil {
    return err
}
```

目前就这些。

# 摘要

在本文中，我们介绍了 TiDB 操作符，从它在`cmd/controller-manager/main.go`中的入口点到控制器的实现，并解释了控制器的内部逻辑。现在，您已经熟悉了控制回路是如何触发的。剩下的唯一问题是如何细化控制回路，并向其中灌输 TiDB 的特殊操作逻辑。这样，您可以根据需要部署 TiDB 并在 Kubernetes 中运行它。

对于想开发资源管理系统的人，我们推荐两个脚手架项目: [Kubebuilder](https://github.com/kubernetes-sigs/kubebuilder) 和 [Operator Framework](https://github.com/operator-framework/operator-sdk) 。这些项目基于[控制器-运行时](https://github.com/kubernetes-sigs/controller-runtime)生成代码模板，允许您关注 CRD 对象的协调循环。

在下一篇文章中，我将讨论细化控制循环和实现组件协调循环。如果您有任何问题，请通过[我们的 Slack 频道](https://slack.tidb.io/invite?team=tidb-community&channel=sig-k8s&ref=pingcap-blog)或通过 [pingcap/tidb-operator](https://github.com/pingcap/tidb-operator) 与我们联系。

*原载于 2021 年 5 月 27 日*[*www.pingcap.com*](https://pingcap.com/blog/tidb-operator-source-code-reading-2-operator-pattern)