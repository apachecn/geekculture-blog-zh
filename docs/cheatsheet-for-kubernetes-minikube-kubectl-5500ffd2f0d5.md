# Kubernetes 的备忘单(MiniKube 和 Kubectl)

> 原文：<https://medium.com/geekculture/cheatsheet-for-kubernetes-minikube-kubectl-5500ffd2f0d5?source=collection_archive---------8----------------------->

By-SANDEEP KUMAR PATEL

![](img/19d6ac20e858ecc1e66861e15d6b48ec.png)

# 迷你库贝

和`kind`一样，`[minikube](https://minikube.sigs.k8s.io/)`是一个让你在本地运行 Kubernetes 的工具。`minikube`在你的个人电脑(包括 Windows、macOS 和 Linux PCs)上运行单节点 Kubernetes 集群，这样你就可以试用 Kubernetes，或者进行日常开发工作。

minikube 在 macOS、Linux 和 Windows 上快速建立本地 Kubernetes 集群。我们自豪地专注于帮助应用程序开发人员和新的 Kubernetes 用户。

# 突出

*   支持最新的 Kubernetes 版本(+6 以前的次要版本)
*   跨平台(Linux、macOS、Windows)
*   作为虚拟机、容器或裸机进行部署
*   多个容器运行时(CRI-O、containerd、docker)
*   超快的 Docker API 端点[图像推送](https://minikube.sigs.k8s.io/docs/handbook/pushing/#pushing-directly-to-the-in-cluster-docker-daemon)
*   高级特性，如[负载平衡器](https://minikube.sigs.k8s.io/docs/handbook/accessing/#loadbalancer-access)、文件系统挂载和特性门
*   [插件](https://minikube.sigs.k8s.io/docs/handbook/deploying/#addons)用于轻松安装 Kubernetes 应用程序

# 1.1 MINIKUBE BASIC

命名命令 minikube 生命周期

`minikube delete`、`minikube start`、`minikube status`、

[链接:minikube](https://github.com/kubernetes/minikube) 获取 minikube 版本

`minikube version`，

[链接:所有 minikube 版本](https://github.com/kubernetes/minikube/releases) mac 安装 minikube

`brew cask install minikube`，`brew cask reinstall minikube`

用不同的机器口味启动 minikube

`minikube start --memory 5120 --cpus=4`

使用特定的 k8s 版本启动 minikube

`minikube start --kubernetes-version v1.11.0`

通过更多自定义启动 minikube minikube start–kubernetes-v 1 . 11 . 0 版–feature-gates = advanced auditing = true ssh to minikube VM

`minikube ssh`，`ssh -i ~/.minikube/machines/minikube/id_rsa docker@192.168.99.100`

您当地的码头工人使用 minikube 码头工人

`eval $(minikube docker-env)`，

那么就不需要`docker push`

Minikube 检查最新版本

`minikube update-check`

# 1.2 检查状态

Name 命令获取 minikube 版本

`minikube version`，

[链接:所有 minikube 版本](https://github.com/kubernetes/minikube/releases)获取集群信息

`kubectl cluster-info`

获取服务信息

`minikube service <srv-name>`

获取仪表板

`minikube dashboard`

获取 ip

`minikube ip`

获取 minikube 日志

`minikube logs`

列表插件

`minikube addons list`

# 1.3 MINIKUBE 文件夹

NameCommandMount 将主机操作系统的文件夹装载到微型虚拟机

`minikube mount /host-mount-path:/vm-mount-path`

k8s.io/minikube-hostpath 置备程序的文件夹

`/tmp/hostpath-provisioner`，`/tmp/hostpath_pv`

将主机操作系统的文件夹装载到 minikube 虚拟机

`minikube mount /host-mount-path:/vm-mount-path`

关键 minikube 文件夹

`/var/lib/localkube`、`/var/lib/docker`、`/data`

检查主机操作系统桌面中的 minikube 配置

`~/.minikube/machines/minikube/config.json`

本地环境中的 Minikube conf

`~/.minikube`，`~/.kube`

# 1.4 MINIKUBE 高级版

创建 minikube env 后的 NameCommandInstall addon

`minikube addons enable heapster`，`kubectl top node`

# 1.5 MINIKUBE CLI 在线帮助

```
> minikube version
minikube version: v0.31.0> minikube --help
Minikube is a CLI tool that provisions and manages single-node Kubernetes clusters optimized for development workflows.Usage:
  minikube [command]Available Commands:
  addons         Modify minikube's kubernetes addons
  cache          Add or delete an image from the local cache.
  completion     Outputs minikube shell completion for the given shell (bash or zsh)
  config         Modify minikube config
  dashboard      Access the kubernetes dashboard running within the minikube cluster
  delete         Deletes a local kubernetes cluster
  docker-env     Sets up docker env variables; similar to '$(docker-machine env)'
  help           Help about any command
  ip             Retrieves the IP address of the running cluster
  logs           Gets the logs of the running instance, used for debugging minikube, not user code
  mount          Mounts the specified directory into minikube
  profile        Profile sets the current minikube profile
  service        Gets the kubernetes URL(s) for the specified service in your local cluster
  ssh            Log into or run a command on a machine with SSH; similar to 'docker-machine ssh'
  ssh-key        Retrieve the ssh identity key path of the specified cluster
  start          Starts a local kubernetes cluster
  status         Gets the status of a local kubernetes cluster
  stop           Stops a running local kubernetes cluster
  tunnel         tunnel makes services of type LoadBalancer accessible on localhost
  update-check   Print current and latest version number
  update-context Verify the IP address of the running cluster in kubeconfig.
  version        Print the version of minikubeFlags:
      --alsologtostderr                  log to standard error as well as files
  -b, --bootstrapper string              The name of the cluster bootstrapper that will set up the kubernetes cluster. (default "kubeadm")
  -h, --help                             help for minikube
      --log_backtrace_at traceLocation   when logging hits line file:N, emit a stack trace (default :0)
      --log_dir string                   If non-empty, write log files in this directory
      --logtostderr                      log to standard error instead of files
  -p, --profile string                   The name of the minikube VM being used.
                                         	This can be modified to allow for multiple minikube instances to be run independently (default "minikube")
      --stderrthreshold severity         logs at or above this threshold go to stderr (default 2)
  -v, --v Level                          log level for V logs
      --vmodule moduleSpec               comma-separated list of pattern=N settings for file-filtered loggingUse "minikube [command] --help" for more information about a command.
```

# 1.6 更多资源

许可证:代码在 [MIT 许可证](https://www.dennyzhang.com/wp-content/mit_license.txt)下获得许可。

[https://github.com/kubernetes/minikube/tree/master/docs](https://github.com/kubernetes/minikube/tree/master/docs)

启动单节点 Kubernetes 集群

```
minikube start --kubernetes-version=<version> --driver=<driver_name>
------
e.g.
minikube start --kubernetes-version=v1.18.0 --driver=dockerMinikube supports the following drivers:
---
docker, virtualbox, podman, vmwarefusion, kvm2, hyperkit, hyperv, vmware, parallels, none
```

获取状态

```
minikube status
```

访问仪表板

```
minikube dashboard
```

停止集群

```
minikube stop
```

删除集群

```
minikube delete
```

列出可用的插件

```
minikube addons list
```

启用加载项

```
minikube addons enable metrics-server
```

禁用加载项

```
minikube addons disable metrics-server
```

群集服务的代理

```
minikube service <service_name>
```

# 库贝特尔

设置 Kubectl 应该引用的集群配置文件

```
kubectl config use-context minikube
```

显示 Kubernetes 客户端和服务器版本

```
kubectl version
```

创建部署

```
kubectl create deployment hello-node --image=k8s.gcr.io/echoserver:1.4
```

查看部署

```
kubectl get deployments
```

查看窗格

```
kubectl get pods
```

查看服务

```
kubectl get services
```

查看窗格和服务

```
kubectl get pod,svc -n kube-system
```

查看事件

```
kubectl get events
```

查看 kubectl 配置

```
kubectl config view
```

将 pod 暴露在公共互联网上

```
kubectl expose deployment hello-node --type=LoadBalancer --port=8080
```

删除服务

```
kubectl delete service hello-node
```

删除部署

```
kubectl delete deployment hello-node
```

# YAML 参考部署

## 示例 1

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: helloworld-web
  labels:
    app: helloworld-web
    tier: frontend
spec:
  replicas: 2
  selector:
    matchLabels:
      app: helloworld-web
      tier: frontend
  template:
    metadata:
      labels:
        app: helloworld-web
        tier: frontend
    spec:
      containers:
      - name: helloworld-web
        image: ssmak/helloworld_web:latest
        ports:
        - containerPort: 80
        - containerPort: 443
```

![](img/18ad860785ad18f6579c6ac9d51b73a5.png)

# 谢谢你……!！！！