# 编译 rpm—Glusterfs

> 原文：<https://medium.com/geekculture/compiling-rpms-36ac24970f0e?source=collection_archive---------13----------------------->

![](img/af2fe7fefc66d44c6faade7023385ede.png)

RPM-Redhat Package Manager

# 内容📑

1.  什么是 RPM？
2.  汇编关于 Fedora，CentOS 和 RHEL 的 rpm。

*   准备步骤
*   常见步骤

# 什么是 RPM？

> **RPM 包管理器** ( **RPM** )(原**红帽包管理器**，现为[递归缩写](https://en.wikipedia.org/wiki/Recursive_acronym))是一个[免费开源的](https://en.wikipedia.org/wiki/Free_and_open-source) [包管理系统](https://en.wikipedia.org/wiki/Package_management_system)。[【5】](https://en.wikipedia.org/wiki/RPM_Package_Manager#cite_note-max-5)RPM 这个名称指的是**。rpm** [文件格式](https://en.wikipedia.org/wiki/File_format)和包管理器程序本身。
> 
> 一个 RPM 包可以包含任意一组文件。大多数 RPM 文件是“二进制 RPM”(或 brpm)，包含一些软件的编译版本。还有包含用于构建二进制包的[源代码](https://en.wikipedia.org/wiki/Source_code)的“源 rpm”(或 SRPMs)。

— [维基百科](https://en.wikipedia.org/wiki/RPM_Package_Manager)💻

# 在 Fedora、Centos 和 RHEL 上编译 rpm

在本文中，我们将在以下操作系统上编译来自 git 源代码的 rpm:

1.  RHEL
2.  百分位 5，6，7，8
3.  软呢帽 16-20，32，33

整个步骤/程序分为两个部分:

1.  准备步骤—特定于操作系统的步骤。
2.  通用步骤—所有操作系统的通用步骤。

## 准备步骤

**用于 Fedora 16–20、32、33**

1.  安装 gcc、python 开发头、python 开发工具

```
# sudo yum -y install gcc python-devel python-setuptools
```

*也可以用****dnf****代替 yum。*

2.如果你正在编译 GlusterFS 版，那么安装 python-swiftclient。其他 GlusterFS 版本不需要它:

```
# sudo easy_install simplejson python-swiftclient
```

对于 Fedora，您可以运行下面的命令来防止与包相关的问题

```
# dnf install automake autoconf libtool flex bison openssl-devel libxml2-devel python-devel libaio-devel libibverbs-devel librdmacm-devel readline-devel lvm2-devel glib2-devel userspace-rcu-devel libcmocka-devel libacl-devel sqlite-devel fuse-devel redhat-rpm-config rpcgen libtirpc-devel make python3-devel rsync libuuid-devel rpm-build libcurl-devel -y
```

现在按照**的通用步骤。**

**用于 Centos 5.x**

1.  安装 **EPEL** 。

```
# curl -OL `[`http://download.fedoraproject.org/pub/epel/5/x86_64/epel-release-5-4.noarch.rpm`](http://download.fedoraproject.org/pub/epel/5/x86_64/epel-release-5-4.noarch.rpm) # sudo yum -y install epel-release-5-4.noarch.rpm --nogpgcheck
```

> EPEL(Enterprise Linux 额外软件包)是一个 Fedora 特别兴趣小组，它为 Enterprise Linux 创建、维护和管理一组高质量的额外软件包，包括但不限于[Red Hat Enterprise Linux](https://fedoraproject.org/wiki/Red_Hat_Enterprise_Linux)(RHEL)、CentOS 和 Scientific Linux (SL)、Oracle Linux (OL)。

2.安装 Centos 5.x 中需要的几个软件包

```
# sudo yum -y install buildsys-macros gcc ncurses-devel \     python-ctypes python-sphinx10 redhat-rpm-config
```

现在转到通用部件部分继续。

**仅适用于 Centos 6 . x**

你需要先安装 EPEL 和一些 Centos 特定的软件包。

```
# sudo yum -y install `[`http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm`](http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm)# sudo yum -y install python-webob1.0 python-paste-deploy1.5 python-sphinx10 redhat-rpm-config
```

现在转到**常用步骤**继续下一步。

**仅适用于 Centos 8 . x**

首先安装 EPEL，然后启用一些 powertools 包。

```
# sudo rpm -ivh [https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm](https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm)# sudo yum --enablerepo=PowerTools install automake autoconf libtool flex bison openssl-devel \     libxml2-devel libaio-devel libibverbs-devel librdmacm-devel readline-devel lvm2-devel \     glib2-devel userspace-rcu-devel libcmocka-devel libacl-devel sqlite-devel fuse-devel \     redhat-rpm-config rpcgen libtirpc-devel make python3-devel rsync libuuid-devel \     rpm-build dbench perl-Test-Harness attr libcurl-devel selinux-policy-devel -y
```

> 在**CentOS**8/**RHEL**8**Linux**上，默认情况下不启用 **PowerTools** 存储库。这个存储库包含许多安装其他应用程序时需要依赖的包，主要是从源代码构建应用程序。

现在从**通用步骤的第 2 步开始。**

**仅适用于 RHEL 6.x 车型**

首先安装 EPEL，然后安装其他软件包。

```
# sudo yum -y install `[`http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm`](http://download.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm)# sudo yum -y --enablerepo=rhel-6-server-optional-rpms install python-webob1.0 \     python-paste-deploy1.5 python-sphinx10 redhat-rpm-config
```

让我们来看看常见的步骤。

# 常见步骤

这些步骤对于 Fedora 和 RHEL/CentOS 来说是常见的。

**以下步骤 1 的注意事项:**

*   如果你在 RHEL/CentOS 5.x 上，得到一条 lvm2-devel 不可用的消息，这没关系。可以忽略。😅
*   如果你在 RHEL/CentOS 6.x 上，得到任何关于 python-eventlet、python-netifaces、python-sphinx 和/或 pyxattr 不可用的消息，没关系。你可以忽略它们。😅
*   如果您使用 CentOS 8.x，可以跳过步骤 1，从步骤 2 开始。此外，对于 CentOS 8.x，这些步骤已针对主分支进行了测试。尚不清楚它是否适用于较老的分支。

**1。安装需要的软件包**

```
# sudo yum -y install git autoconf \     automake bison dos2unix flex fuse-devel glib2-devel libaio-devel \     libattr-devel libibverbs-devel librdmacm-devel libtool libxml2-devel lvm2-devel make \     openssl-devel pkgconfig pyliblzma python-devel python-eventlet python-netifaces \     python-paste-deploy python-simplejson python-sphinx python-webob pyxattr readline-devel \     rpm-build systemtap-sdt-devel tar libcmocka-devel 
```

**2。克隆**[**glusterfs repo**](https://github.com/gluster/glusterfs)**和 cd 到里面。**

```
# git clone <web-url/ssh>
# cd glusterfs
```

**3。选择编译哪个分支**

如果要编译最新的开发代码，可以跳过这一步。

要编译特定版本:

*   列出分支

```
# git branch -a | grep release 
**remotes/origin/release-2.0 
remotes/origin/release-3.0 
remotes/origin/release-3.1 
remotes/origin/release-3.2 
remotes/origin/release-3.3 
remotes/origin/release-3.4 
remotes/origin/release-3.5**
```

*   切换到 glusterfs 特定版本的分支

```
# git checkout <branch>
Ex: 
# git checkout **release-3.4**
```

**注**—CentOS 5 . x 指令仅针对 GlusterFS git 中的主分支进行了测试。目前还不知道它们是否适用于版本 3.5 之前的分支。

**4。配置并编译 GlusterFS**

```
# cd extras/LinuxRPM 
# ./make_glusterrpms
```

搞定了。😃

```
# ls -l *rpm
```

您将在您的文件夹中获得 rpm 列表。

```
# ls -l *rpm 
-rw-rw-r-- 1 jc jc 3966111 Mar  2 12:15 glusterfs-3git-1.el5.centos.src.rpm 
-rw-rw-r-- 1 jc jc 1548890 Mar  2 12:17 glusterfs-3git-1.el5.centos.x86_64.rpm 
-rw-rw-r-- 1 jc jc   66680 Mar  2 12:17 glusterfs-api-3git-1.el5.centos.x86_64.rpm 
-rw-rw-r-- 1 jc jc   20399 Mar  2 12:17 glusterfs-api-devel-3git-1.el5.centos.x86_64.rpm 
-rw-rw-r-- 1 jc jc  123806 Mar  2 12:17 glusterfs-cli-3git-1.el5.centos.x86_64.rpm 
-rw-rw-r-- 1 jc jc 7850357 Mar  2 12:17 glusterfs-debuginfo-3git-1.el5.centos.x86_64.rpm 
-rw-rw-r-- 1 jc jc  112677 Mar  2 12:17 glusterfs-devel-3git-1.el5.centos.x86_64.rpm 
-rw-rw-r-- 1 jc jc  100410 Mar  2 12:17 glusterfs-fuse-3git-1.el5.centos.x86_64.rpm 
-rw-rw-r-- 1 jc jc  187221 Mar  2 12:17 glusterfs-geo-replication-3git-1.el5.centos.x86_64.rpm 
-rw-rw-r-- 1 jc jc  299171 Mar  2 12:17 glusterfs-libs-3git-1.el5.centos.x86_64.rpm 
-rw-rw-r-- 1 jc jc   44943 Mar  2 12:17 glusterfs-rdma-3git-1.el5.centos.x86_64.rpm 
-rw-rw-r-- 1 jc jc  123065 Mar  2 12:17 glusterfs-regression-tests-3git-1.el5.centos.x86_64.rpm 
-rw-rw-r-- 1 jc jc   16224 Mar  2 12:17 glusterfs-resource-agents-3git-1.el5.centos.x86_64.rpm 
-rw-rw-r-- 1 jc jc  654043 Mar  2 12:17 glusterfs-server-3git-1.el5.centos.x86_64.rpm
```

# 参考

 [## Gluster 文档

### GlusterFS 是一个可扩展的网络文件系统，适用于数据密集型任务，如云存储和媒体流…

docs.gluster.org](https://docs.gluster.org/en/latest/) 

# 也阅读

[](https://ujjwal-msrit.medium.com/gluster-geo-replication-83dab508a9de) [## Gluster —地理复制

### 什么是地理复制？

ujjwal-msrit.medium.com](https://ujjwal-msrit.medium.com/gluster-geo-replication-83dab508a9de) [](https://ujjwal-msrit.medium.com/gluster-directory-quotas-ebdf95bbfef6) [## Gluster —目录配额

### 什么是配额？

ujjwal-msrit.medium.com](https://ujjwal-msrit.medium.com/gluster-directory-quotas-ebdf95bbfef6)