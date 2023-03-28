# 如何使用 Terraform 和 Ansible 将 Kubernetes 应用程序部署到 AWS EKS？

> 原文：<https://medium.com/geekculture/how-to-deploy-the-kubernetes-application-to-aws-eks-using-terraform-and-ansible-7679a3a207a8?source=collection_archive---------0----------------------->

# 什么是 AWS EKS？

**亚马逊弹性 Kubernetes 服务**(亚马逊 EKS)是由 AWS 提供的托管 Kubernetes 服务。通过 **AWS EKS** 我们可以在不安装和操作 Kubernetes 控制平面或工作节点的情况下运行 Kubernetes。 **AWS EKS** 帮助您提供高度可用和安全的集群，并自动执行关键任务，如打补丁、节点配置和更新。

![](img/d4876e9648d8f6dcd1df49225f669bd8.png)

AWS EKS

# 什么是 Terraform？

**Terraform** 是一个免费的开源**基础设施 as code (IAC)** ，可以帮助自动化远程服务器的部署、配置和管理。Terraform 可以管理现有的服务提供商和定制的内部解决方案。

从 [**这里**](https://www.terraform.io/intro/index.html) **阅读更多关于 Terraform 的内容。**

![](img/da61d3b54a8bed5626f5bb75b6aa0924.png)

Terraform

# 什么是 Ansible？

Ansible 是一款开源软件**供应、配置管理和部署工具。它运行在许多类 Unix 系统上，可以配置类 Unix 系统和微软视窗系统。Ansible 使用 SSH 协议来配置远程服务器。Ansible 遵循基于推的机制来配置远程服务器。**

![](img/0f6b157303e87f1cc699a7b5eee70e3e.png)

Ansible

从 [**这里**](https://docs.ansible.com/) **阅读更多关于 Ansible 的内容。**

本教程分为 **2 部分**。

1.  使用**地形**创建 Kubernetes 集群
2.  使用 **Ansible** 部署 Kubernetes 应用程序

# 先决条件:

*   **AWS 账户**
*   基本了解 **AWS** 、 **Terraform、ansi ble**&**Kubernetes**
*   **GitHub 账户**

![](img/39d3ee28dd50364464e70a03a19b7263.png)

Terraform + Ansible + AWS EKS

## **第 1 部分:-****Kubernetes 集群的地形脚本。**

**第一步:-** 创建**。用于存储环境变量的 tf** 文件

*   创建`vars.tf`文件并在其中添加以下内容

```
variable "access_key" {
  default = "<Your-AWS-Access-Key>"
}variable "secret_key" {
    default = "<Your-AWS-Secret-Key>"
}
```

**第二步:-** 创建**。AWS 配置的 tf** 文件

*   创建`main.tf`文件并在其中添加以下内容

```
provider "aws" {
    region = "us-east-1"
    access_key = "${var.access_key}"
    secret_key = "${var.secret_key}"
}data "aws_availability_zones" "azs" {
    state = "available"
}
```

*   `data “aws_availability_zones” “azs”`将提供`us-east-1`区域的可用区域列表

**第三步:-** 创建**。 **AWS VPC** 的 tf** 文件

*   为 **VPC** 创建`vpc.tf`文件，并在其中添加以下内容

```
variable "region" {
    default = "us-east-1"
}data "aws_availability_zones" "available" {}locals {
    cluster_name = "EKS-Cluster"
}module vpc {
    source = "terraform-aws-modules/vpc/aws"
    version = "3.2.0"
    name = "Demo-VPC"
    cidr = "10.0.0.0/16"
    azs = data.aws_availability_zones.available.names
    private_subnets = ["10.0.1.0/24", "10.0.2.0/24", "10.0.3.0/24"]
    public_subnets =  ["10.0.4.0/24", "10.0.5.0/24", "10.0.6.0/24"]
    enable_nat_gateway = true
    single_nat_gateway = true
    enable_dns_hostname = truetags = {
    "Name" = "Demo-VPC"
}public_subnet_tags = {
    "Name" = "Demo-Public-Subnet"
}private_subnet_tags = {
    "Name" = "Demo-Private-Subnet"
}
}
```

*   我们正在使用 **AWS VPC** 模块进行 VPC 创作
*   上面的代码将创建`us-east-1`地区`10.0.0.0/16` CIDR 范围的 **AWS VPC**
*   **VPC** 将有 3 个**公共**和**私有**子网
*   `data “aws_availability_zones” “azs”`将提供`us-east-1`地区的可用区域列表
*   我们已经启用了`NAT Gateway` & `DNS Hostname`

**第四步:-** 创建。 **AWS 安全组**的 tf 文件

*   为 **AWS 安全组**创建`security.tf`文件，并在其中添加以下内容

```
resource "aws_security_group" "worker_group_mgmt_one" {
    name_prefix = "worker_group_mgmt_one"
    vpc_id = module.vpc.vpc_idingress {
        from_port = 22
        to_port = 22
        protocol = "tcp"cidr_blocks = [
            "10.0.0.0/8"
        ]
    }
}resource "aws_security_group" "worker_group_mgmt_two" {
    name_prefix = "worker_group_mgmt_two"
    vpc_id = module.vpc.vpc_id

    ingress {
        from_port = 22
        to_port = 22
        protocol = "tcp"cidr_blocks = [
            "10.0.0.0/8"
        ]
    }
}resource "aws_security_group" "all_worker_mgmt" {
    name_prefix = "all_worker_management"
    vpc_id = module.vpc.vpc_idingress {
        from_port = 22
        to_port = 22
        protocol = "tcp"cidr_blocks = [
            "10.0.0.0/8"
        ]
    }
}
```

*   我们正在为 2 个工作节点组创建 2 个安全组
*   我们只允许 **22** 端口用于 **SSH** 连接
*   我们正在限制对`10.0.0.0/8` CIDR 数据块的 SSH 访问

**第五步:-** 创建**。 **EKS 集群**的 tf** 文件

*   为 **VPC** 创建`eks.tf`文件，并在其中添加以下内容

```
module "eks"{
    source = "terraform-aws-modules/eks/aws"
    version = "17.1.0"
    cluster_name = local.cluster_name
    cluster_version = "1.20"
    subnets = module.vpc.private_subnetstags = {
        Name = "Demo-EKS-Cluster"
    }vpc_id = module.vpc.vpc_id
    workers_group_defaults = {
        root_volume_type = "gp2"
    }workers_group = [
        {
            name = "Worker-Group-1"
            instance_type = "t2.micro"
            asg_desired_capacity = 2
            additional_security_group_ids = [aws_security_group.worker_group_mgmt_one.id]
        },
        {
            name = "Worker-Group-2"
            instance_type = "t2.micro"
            asg_desired_capacity = 1
            additional_security_group_ids = [aws_security_group.worker_group_mgmt_two.id]
        },
    ]
}data "aws_eks_cluster" "cluster" {
    name = module.eks.cluster_id
}data "aws_eks_cluster_auth" "cluster" {
    name = module.eks.cluster_id
}
```

*   对于 EKS 集群的创建，我们使用的是 terraform AWS EKS 模块
*   下面的代码将创建 2 个 worker 组，具有所需的 3 个类型为 **t2.micro** 的实例的容量
*   我们将最近创建的安全组附加到两个工作节点组

```
workers_group = [
        {
            name = "Worker-Group-1"
            instance_type = "t2.micro"
            asg_desired_capacity = 2
            additional_security_group_ids = [aws_security_group.worker_group_mgmt_one.id]
        },
        {
            name = "Worker-Group-2"
            instance_type = "t2.micro"
            asg_desired_capacity = 1
            additional_security_group_ids = [aws_security_group.worker_group_mgmt_two.id]
        },
    ]
```

**第六步:-** 创建**。terraform **Kubernetes** 提供者的 tf** 文件

*   创建`kubernetes.tf`文件并在其中添加以下内容

```
provider "kubernetes" {
    host = data.aws_eks_cluster.cluster.endpoint
    token = data.aws_eks_cluster_auth.cluster.token
    cluster_ca_certificate = base64encode(data.aws_eks_cluster.cluster.certificate_authority.0.data)
}
```

*   在上面的代码中，我们使用最近创建的集群作为`host`和认证令牌作为`token`
*   我们将`cluster_ca_certificate`用于 **CA 证书**

**第七步:-** 创建**。**输出**的 tf** 文件

*   创建`outputs.tf`文件并在其中添加以下内容

```
output "cluster_id" {
    value = module.eks.cluster_id
}output "cluster_endpoint" {
    value = module.eks.cluster_endpoint
}
```

*   上面的代码将输出我们集群的名称，并公开我们集群的端点。

**步骤 8:-** 将我们的代码存储到 **GitHub 仓库**

*   现在，我们已经完成了所有的 terraform 脚本，让我们将代码存储在 GitHub 存储库中

![](img/bc828f2c538c9b54e8e23ae79b542739.png)

GitHub Repository

步骤 9:-初始化工作目录

*   在工作目录下运行`terraform init`命令。它将下载所有必需的提供程序和所有模块

**步骤 10:-** 创建一个**地形图**

*   在工作目录下运行`terraform plan`命令。它会给出执行计划

```
Plan: 50 to add, 0 to change, 0 to destroy.Changes to Outputs:+ cluster_endpoint = (known after apply)+ cluster_id       = (known after apply)
```

**步骤 11:-** 在 **AWS** 上创建**集群**

*   运行工作目录中的`terraform apply`命令。它将在 **AWS** 上创建 Kubernetes 集群
*   Terraform 将在 AWS 上创建以下资源

1.  **VPC**
2.  **路由表**
3.  **我的角色**
4.  **NAT 网关**
5.  **安全组**
6.  **公共&私有子网**
7.  **EKS 集群**

**第 12 步:-** 验证 **AWS** 上的资源

*   导航到您的 **AWS** 账户并验证资源

1.  **EKS 集群:**

![](img/6ffd5c9d17932f7bb0b5728c99295029.png)

AWS EKS

![](img/1a62df4f459f9233c7388bf99663e3df.png)

AWS EKS

**2。VPC &其他资源:**

![](img/ba5f0cd19d42d825fa29be93649e9ca4.png)

AWS VPC

**3。子网:**

![](img/38e759fa7d985da5c2fbdd380099e737.png)

AWS Subnets

**4。安全组:**

![](img/21342725782f5557398a497d7ea27c16.png)

AWS Security Group

**5。IAM 角色:**

![](img/d3a9484f11ae370d72cf7f53e66dc5ff.png)

AWS IAM Role

**6。自动缩放组:**

![](img/fb55b4b37275c186b3e27986adfe6434.png)

AWS ASG

**7。EC2 实例:**

![](img/ab58d0bf8e65a98de50de54dd9b568c6.png)

AWS EC2 Instances

现在，我们的 Kubernetes 集群已经准备好了，让我们开始为我们的应用程序创建代码。

在上面的例子中，我正在管理底层的 **EKS 工人组织的 EC2 实例。因此，我可以随心所欲地修改实例。**

## 第 2 部分:Kubernetes 应用的可行方案

**步骤 1:-** 为 **Pod 定义**创建`.yml`文件

*   在下面的代码中，我使用了`dhsoni-web`图像，即我的投资组合网站的图像。您也可以选择任何其他图像。

```
apiVersion: v1
kind: Pod
metadata:
  name: mywebsite-pod
  labels:
    app: mywebsite
spec:
  containers:
    - name: mywebsite-container
      image: dhruvin30/dhsoniweb
```

**第二步:** -为**服务定义**创建`.yml`文件

*   创造。yml 文件，并将下面的代码添加到其中。它将创建类型为`loadbalancer`的服务

```
apiVersion: v1
kind: Service
metadata:
  name: mywebsite-svc
  labels:
    app: mywebsite-svc
spec:
  ports:
  - port: 80
    protocol: TCP
  selector:
    app: mywebsite-svc
  type: LoadBalancer
```

**第三步:-** 为**可行游戏**创建`.yml`文件

*   创建`.yml`文件，并在其中添加以下代码。它将为 Kubernetes 应用程序创建 **Pod** 和**服务**。

```
---- name: Deploy to K8s Cluster 
  hosts: all
  become: truetasks:
  - name: Deploy Pod
    shell: |
      kubectl apply -f pod.yml

  - name: Deploy Service
    shell: | 
      kubectl apply -f svc.yml
```

*   运行以下命令以配置应用程序

```
ansible-playbook <playbookname.yml>
```

*   播放完成后，您可以通过访问网络浏览器上的`ec2-ip:80`来查看应用程序。您应该会看到如下输出

![](img/2e67e7c11fc4e0ba38f1ba3626883fb6.png)

Website

现在就这样，你已经学会了如何使用 **Terraform** 创建 **AWS EKS** 集群，如何使用 **Ansible 创建 **Kubernetes** 应用程序。**您现在可以使用它并进行相应的修改。

你可以在我的 [**GitHub**](https://github.com/DhruvinSoni30/Terraform-EKS-Ansible) 账号找到完整的代码。也可以随意查看我的其他库。

如果您觉得本指南很有帮助，请点击👏按钮，也可以随意发表评论。

关注更多类似的故事😊