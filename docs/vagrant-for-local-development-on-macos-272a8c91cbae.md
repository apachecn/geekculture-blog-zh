# 使用虚拟机进行开发的 macOS 上的 Linux 环境。

> 原文：<https://medium.com/geekculture/vagrant-for-local-development-on-macos-272a8c91cbae?source=collection_archive---------4----------------------->

![](img/b08700ab1267bfd959caf02ef280c51a.png)

Vagrant

在将您的功能部署到服务器之前，本地开发环境在测试您的功能方面扮演着重要的角色。在需要不同配置和环境的本地机器中创建所有运行模块的完全集成环境是很难实现的。例如，在 macOS 中测试项目的完全集成，而有些模块需要 linux 环境。有一些方法可以通过创建模拟来测试你的模块，但是它们不提供完全的集成测试。

在一个大型的工业项目中工作，需要多个模块聚集在一起测试一个变更，在服务器上验证一个变更将会成为一个乏味的任务。一个完全集成的本地开发环境在部署到服务器上之前对测试变化起着很大的作用。创建虚拟机提供了在本地主机操作系统中测试所需的不同环境的好处。

**流浪者**提供了一种有效的方式来供应虚拟机。使用配置文件，在配置时启动虚拟机并安装所需的软件包。让我们来看看 lavour 的工作方式，并使用 lavour file 为虚拟机提供所需的软件包

开始使用虚拟机的流浪者所需的上述步骤:-

*   将 oracle [虚拟盒](https://www.oracle.com/in/virtualization/technologies/vm/downloads/virtualbox-downloads.html)下载并安装到本地 mac os 机器上
*   在本地 mac os 机器上下载并安装 vagger，也可以使用`brew install vagrant`。
*   创建一个文件夹(vacator/<project-name>)来保存 vacator file(创建一个包含用于配置虚拟机的指令集的 vacator file)。例如:-</project-name>

Vagrantfile

**浮动文件**用于向虚拟机提供上述文件中定义的指令集。该文件可用于为多个虚拟机调配不同的资源。在上面的文件中需要注意的是，我已经提供了一个 nfs 挂载卷共享，然后在配置 VM 时将指令添加到了一个选项参数中

```
Vagrant.configure(2) **do** |config|
   opts = {
          type: 'nfs',
          linux__nfs_options: ['no_root_squash'],
          map_uid: 0,
          map_gid: 0
   }
   config.vm.synced_folder "/Users/himanshuchaudhary/pds_nvme_release_drop1", "/root/pds_nvme_release", opts
```

在为 nfs 共享装载提供选项后，您可以将本地卷装载到将被同步的 After。因此，您在本地主机的目录中所做的任何更改都将反映在 VM 中。使用 nfs 装载选项的另一个好处是当您需要卷装载来运行 docker 容器时。docker 的 mount 选项有助于满足许多需求，其中之一是当您希望项目细节即使在 VM 被销毁后也能持久化时。

上面文件中的另一行是创建一个从本地主机网络到 VM 内部网络的私有网络接口桥。提供专用网络的好处是，如果您连接到网络，除了您的本地主机之外，它不会暴露给其他机器。

```
Vagrant.configure(2) **do** |config|
config.vm.network "private_network", ip: "192.168.56.9"
```

如果你想为整个网络创建一个网络接口，有一个选项可以在流浪者文件中创建公共网络。

一旦游民文件准备好了，运行`vagrant up`来供应虚拟机。它将添加虚拟机所需的配置，包括调配计算资源。如果是 mac OS，当挂载 nfs 共享时，它会要求 root 密码(管理员密码)。

```
==> default: Installing NFS client...
==> default: Exporting NFS shared folders...
==> default: Preparing to edit /etc/exports. Administrator privileges will be required...
```

提供管理员密码后，你的流浪者将被配置和运行。使用`vagrant ssh`ssh 进入正在运行的虚拟机。当您通过 ssh 进入系统时，您可以在终端上看到配置和专用网络的详细信息

```
Welcome to Ubuntu 20.04.3 LTS (GNU/Linux 5.4.0-91-generic x86_64)* Documentation:  [https://help.ubuntu.com](https://help.ubuntu.com)
 * Management:     [https://landscape.canonical.com](https://landscape.canonical.com)
 * Support:        [https://ubuntu.com/advantage](https://ubuntu.com/advantage)System information as of Mon May 30 10:26:56 UTC 2022System load:  0.0               Processes:               163
  Usage of /:   9.6% of 38.71GB   Users logged in:         0
  Memory usage: 6%                IPv4 address for enp0s3: 10.0.2.15
  Swap usage:   0%                IPv4 address for enp0s8: 192.168.56.9
```

现在，您已经有了一个使用 vagrant 启动并运行的虚拟机，该虚拟机可用于测试运行在虚拟机内部并从本地 gRPC macOS 客户端使用的 gRPC 服务器。或者测试 REST 微服务，一部分在本地运行，另一部分在虚拟机运行。还有其他好处，在下面的要点中描述。

流浪者提供命令来管理运行的虚拟机以及虚拟机`vboxmanage`命令:-

*   要列出流浪者，您可以运行`vagrant box list`。一旦你有了流浪者的名字，你就可以用它来删除它或者获取运行虚拟机的详细信息。
*   在流浪者运行`vagrant box update`中更新新版本的操作系统镜像。
*   要移除虚拟机，请使用`vagrant box remove -f.`

对 VM 使用 vagrant 的好处:-

*   它提供了一个单独的浮动文件来为所有系统创建一个开发环境，这是团队工作时所需要的。
*   易于使用，因为所有配置都可以在单个文件中捕获，该文件可用于为虚拟机提供在 Vagrantfile 中提到的所有软件包以供使用。
*   在本地机器上安装项目所需的所有软件会使任务变得繁重，并且这些过程会消耗多种资源。在虚拟机中安装它们可以避免这种情况。

还有其他好处，包括虚拟机设置提供的好处。值得注意的是，可以使用 vagger 来配置多个具有相同配置的虚拟机。流浪者文件将有包和配置在一个地方，可以根据您的项目需求随时修改。使用单个文件来创建开发环境避免了维护包的列表，并使跟踪需要安装的版本变得容易。

> ***如何到达*** *:-邮件至****:*himanshusdec8@gmail.com***如有任何关于实施云或什么是云的相关疑问或讨论。*

拍手声👏如果你今天学到了什么好东西。关注 [Himanshu Chaudhary](https://medium.com/u/64edd4213f30?source=post_page-----272a8c91cbae--------------------------------) 获取更多关于云和云技术的文章。谢谢读者。🙂

参考文件的链接:-

*   https://www.vagrantup.com/docs
*   【https://www.virtualbox.org/wiki/Technical_documentation 