# 使用 EC2、ECS 和 Fargate 对 YCSB 进行简单的 MongoDB 负载测试

> 原文：<https://medium.com/geekculture/simple-mongodb-load-testing-with-ycsb-using-ec2-ecs-and-fargate-343f828f53fb?source=collection_archive---------9----------------------->

MongoDB 是市场上最流行的文档数据库之一。从运营的角度来看，我们有很多不同的选项来设置 MongoDB。为了进行简单的比较，我们将在 AWS 上下文中进行比较，以测试哪个选项最适合您设置 MongoDB。在工具方面，我们将使用来自 Yahoo 的流行的 YCSB 来运行负载测试。由于本文没有演示 Terraform 脚本，如果需要，我们将简单地复制该脚本。不过，欢迎你来探索我的地形设置。

# 先决条件

如果你喜欢按照设置来，你可以查看一下这个[源代码](https://github.com/jazztong/mongo-load-test-tf)，自己准备下面的环境和工具。

1.  Visual Studio 代码(首选)
2.  AWS 帐户(如果您没有)
3.  在本地设置 AWS 帐户
4.  安装 Terraform (0.14.9+)

# 高级图表

一切都要从一张图开始。下图解释了整个设置。

![](img/f38f8abffe02928d40cd64ed61100541.png)

YCSB MongoDB test setup

# 设置说明

我们将设置 4 组不同的堆栈，它们是:-

## 测试客户

EC2 客户端安装 YCSB 来加载 MongoDB，MongoDB 运行在 M5。大型实例类型以避免 CPU 瓶颈。

## Mongo-EC2 服务

与 M5 一起在 EC2 中运行的 MongoDB 服务。具有 1000 IOPS IO1 卷的大型实例类型和 EBS 根存储。

## 蒙戈-法尔盖特服务公司

一个运行在 ECS Fargate 中的 MongoDB 服务，具有 2 个 vCPU 和 8GB Ram。它附有 EFS 卷，用于永久存储。

## Mongo-ECS 服务

在 ECS 中运行的 MongoDB 服务，具有 EC2 启动类型。EC2 与 M5 建立了联系。具有 1000 IOPS IO1 卷的大型实例类型和 EBS 根存储。

# MongoDB 版本

MongoDB 社区版将在所有测试中使用。

# YCSB 设置

[YCSB](https://github.com/brianfrankcooper/YCSB) 代表**雅虎云服务基准**。它用于对各种非 SQL 数据库进行基准测试。它有不同类型的工作负载来执行测试。为了简单起见，我们将只执行一个负载测试工作负载。我们将运行 100 个并发线程，测试 500，000 条记录的数据，其中 1 条记录的平均大小为 1KB。下面的 YCSB 命令用于执行加载。

```
./bin/ycsb load mongodb -s -P workloads/workloada -p recordcount=500000 -threads 100 -p mongodb.url="mongodb://root:mypassword@mongo-ec2.loadtest.internel:27017/admin"
```

# 检验并构建您的环境

下载源代码，用 Visual Studio 代码打开它。你应该得到这样的东西。

![](img/30f330932ea2bf66fc9b978d40b1af01.png)

运行以下 terraform 命令来配置基础架构。

```
export AWS_DEFAULT_REGION=ap-southeast-1
terraform init
terraform plan
terraform apply --auto-approve
```

您将获得以下输出结果:-

![](img/cfd3a6e5c702f9e14519b8c7275c3776.png)

# 使用 SSM 登录并执行加载

为了方便起见，所有 ec2 都配置为允许 SSM 登录。你可以参考这本[SSM 指南](https://docs.aws.amazon.com/systems-manager/latest/userguide/session-manager-working-with-sessions-start.html#start-ec2-console)。

在 AWS 控制台中打开您的 EC2 仪表板，查看如下列表:-

![](img/938dd20537ed270078071e5531fe824f.png)

使用 SSM 连接名为 **testclient-ec2-load-test** 的 ec2

![](img/ab12e925c117604bdc2c2ea46b5fde03.png)![](img/1f0e01bbd9da8d0a6c141f0e667a6fcb.png)

登录 SSM 浏览器外壳后，使用命令`sudo su`切换到超级用户以运行 YCSB 命令。

# 运行 Mongo EC2 负载测试

在 SSM 控制台中，使用以下命令运行 Mongo EC2 负载测试。因为专用 DNS 被有意配置为对所有 DNS 保持相同，所以您只需复制并运行:-

```
cd /ycsb-0.17.0./bin/ycsb load mongodb -s -P workloads/workloada -p recordcount=500000 -threads 100 -p mongodb.url="mongodb://root:mypassword@mongo-ec2.loadtest.internel:27017/admin"
```

您将获得以下结果:-

```
[OVERALL], RunTime(ms), 48170[OVERALL], Throughput(ops/sec), 10379.904504878556[TOTAL_GCS_PS_Scavenge], Count, 136[TOTAL_GC_TIME_PS_Scavenge], Time(ms), 325[TOTAL_GC_TIME_%_PS_Scavenge], Time(%), 0.6746937928171061[TOTAL_GCS_PS_MarkSweep], Count, 0[TOTAL_GC_TIME_PS_MarkSweep], Time(ms), 0[TOTAL_GC_TIME_%_PS_MarkSweep], Time(%), 0.0[TOTAL_GCs], Count, 136[TOTAL_GC_TIME], Time(ms), 325[TOTAL_GC_TIME_%], Time(%), 0.6746937928171061[CLEANUP], Operations, 100[CLEANUP], AverageLatency(us), 32.24[CLEANUP], MinLatency(us), 0[CLEANUP], MaxLatency(us), 3167[CLEANUP], 95thPercentileLatency(us), 1[CLEANUP], 99thPercentileLatency(us), 14[INSERT], Operations, 500000[INSERT], AverageLatency(us), 9436.848092[INSERT], MinLatency(us), 150[INSERT], MaxLatency(us), 6619135[INSERT], 95thPercentileLatency(us), 18207[INSERT], 99thPercentileLatency(us), 46047[INSERT], Return=OK, 500000
```

# 运行 Mongo Fargate 负载测试

执行以下命令对 Mongo Fargate 运行负载测试:-

```
./bin/ycsb load mongodb -s -P workloads/workloada -p recordcount=500000 -threads 100 -p mongodb.url="mongodb://root:mypassword@mongo-fargate.fargate.load:27017/admin"
```

你会得到以下结果:-

```
[OVERALL], RunTime(ms), 74961[OVERALL], Throughput(ops/sec), 6670.135136937874[TOTAL_GCS_PS_Scavenge], Count, 203[TOTAL_GC_TIME_PS_Scavenge], Time(ms), 417[TOTAL_GC_TIME_%_PS_Scavenge], Time(%), 0.5562892704206187[TOTAL_GCS_PS_MarkSweep], Count, 0[TOTAL_GC_TIME_PS_MarkSweep], Time(ms), 0[TOTAL_GC_TIME_%_PS_MarkSweep], Time(%), 0.0[TOTAL_GCs], Count, 203[TOTAL_GC_TIME], Time(ms), 417[TOTAL_GC_TIME_%], Time(%), 0.5562892704206187[CLEANUP], Operations, 100[CLEANUP], AverageLatency(us), 30.54[CLEANUP], MinLatency(us), 0[CLEANUP], MaxLatency(us), 2959[CLEANUP], 95thPercentileLatency(us), 2[CLEANUP], 99thPercentileLatency(us), 8[INSERT], Operations, 500000[INSERT], AverageLatency(us), 13731.694948[INSERT], MinLatency(us), 1056[INSERT], MaxLatency(us), 18743295[INSERT], 95thPercentileLatency(us), 28191[INSERT], 99thPercentileLatency(us), 67647[INSERT], Return=OK, 500000
```

# 运行 Mongo ECS 负载测试

执行下面的命令，对带有 EC2 启动类型的 Mongo ECS 运行负载测试:-

```
./bin/ycsb load mongodb -s -P workloads/workloada -p recordcount=500000 -threads 100 -p mongodb.url="mongodb://root:mypassword@mongo-ecs.ecs.load:27017/admin"
```

您将获得以下结果:-

```
[OVERALL], RunTime(ms), 45572[OVERALL], Throughput(ops/sec), 10971.64925831651[TOTAL_GCS_PS_Scavenge], Count, 136[TOTAL_GC_TIME_PS_Scavenge], Time(ms), 304[TOTAL_GC_TIME_%_PS_Scavenge], Time(%), 0.6670762749056438[TOTAL_GCS_PS_MarkSweep], Count, 0[TOTAL_GC_TIME_PS_MarkSweep], Time(ms), 0[TOTAL_GC_TIME_%_PS_MarkSweep], Time(%), 0.0[TOTAL_GCs], Count, 136[TOTAL_GC_TIME], Time(ms), 304[TOTAL_GC_TIME_%], Time(%), 0.6670762749056438[CLEANUP], Operations, 100[CLEANUP], AverageLatency(us), 36.33[CLEANUP], MinLatency(us), 0[CLEANUP], MaxLatency(us), 3517[CLEANUP], 95thPercentileLatency(us), 2[CLEANUP], 99thPercentileLatency(us), 64[INSERT], Operations, 500000[INSERT], AverageLatency(us), 8879.413404[INSERT], MinLatency(us), 149[INSERT], MaxLatency(us), 6688767[INSERT], 95thPercentileLatency(us), 18639[INSERT], 99thPercentileLatency(us), 56287[INSERT], Return=OK, 500000
```

# 重新运行并清理

如果希望重新运行负载测试，必须删除每次创建的 YCSB 数据库。在您对测试感到满意之后，您可以运行`terraform destroy --auto-approve`来清理一切。

# 摘要

下表显示了 3 种设置之间的比较。

![](img/9c34ae513abf57ad77a7f2f979530d48.png)

一个明显的比较是 Fargate 与 EFS 设置在写入磁盘时会产生更多的延迟。这反映了 AWS [声称](https://docs.aws.amazon.com/efs/latest/ug/performance.html)与 EBS 相比，EFS 的每次操作延迟更长。如果使用 EC2 设置作为基本指标，EFS 的 Fargate 具有 **55%** 更高的延迟。同时，EC2 启动类型的 ECS 具有相同的延迟。

根据测试结果，建议在基本 EC2 或 ECS 中设置 MongoDB，使用 EC2 启动类型而不是 Fargate。希望 AWS 将在 Fargate 启动类型中宣布 EBS 支持，以简化 docker 管理，这需要低延迟的持久存储。